title: AWS - Storage Gateway best practise
date: 2018-04-26 09:13:23
tags:
- AWS Best practise
- Storage Gateway
---


# Introduction

使用storage gateway帮助实现Hibrid Architecture

* 使用gateway实现 NFS到Amazon S3的转换：翻译NAS协议到S3到API call
* 云端的AWS EMR或者Athena可以直接访问备份到S3的数据

# Architecture

* Storage Gateway是virtual applicance
* 支持NFS v3.0 or 4.1
* local storage is used to provide read/write cache
* __"Bucket Share"__: 一个share代表一个S3到NFS的mount point映射 （s3到bucket share是一对一的关系）
* 一个gateway至多支持10个bucket share

# File to object mapping

* 通常unix文件的读写权限（owner，group， permission）和时间戳会映射到s3d的object的metadata中

# Read/Write操作和本地cache

* LRU（least recently used）算法用来evict data
* Read Operation （read through cache）， 先读cache，没有则访问网络
* Write Operation （write-back cache）， 先写cache（parallel writes到local），然后异步写变化的部分回网络

# NFS Security in LAN

* Mounted file system will have default Unix permission defined
* Can define based on source hostname or range of hostname (by define CIDR or by individual IP addresses)

# Monitoring Cache and Traffic

* file gateway will provide statistical info to Amazon Cloudwatch metrics
* cover: cache consumption, cache hits/misses, data transfer and read/write


# File Gateway Bucket Inventory

* if one bucket is associated with more than one gateway, then gateway A don't aware of changes happending in gateway B
* if S3 object is modifed outside NFS share, gateway must RefreshCache (via API or CLI) to pick up the changes

# Bucket Shares with multi Contributors

* there's NO LOCKING or COHERENCY accorss file gatways
* the change will be picked up
   * if the object is only being listed but not yet been queried (not loaded) when change happens, then it will be loaded when first being queried
   * if the gateway "RefreshCache" API / CLI is executed
* Best Practise: one writer gateway, multi reader gateway
   * there's a "ready-only" mount option


# Amazon S3 and file Gateway

* AWS file gateway
  * 可以接 S3或者S3-IA

* S3和S3-IA都可以定义lifecycle policy： 基于object的创建时间或者tag的key value pair

* S3
  * 小数点后9个9的高可用性
  * layer1，可以往S3-IA或者Glacier导

* S3 - IA (infrequence access)
  * 小数点后2个9的高可用性
  * 适合存超过30天大约128KB的object

* Glacier

## Transition

* object转到S3-IA的时候对于file gateway来说还是可见的（read/write）
* object根据lifecycle转到Glacier后就只能list了，不可以读写。（I/O error）

## 结合S3的CRR (Cross Region Replication)


# 使用案例

## Cloud Tiering

利用S3的灵活性，增强现有数据中心存储的durability；用多少花多少；无线的虚拟存储扩展；以及low latency via gateway

## Hybrid Cloud Backup

* gateway提供一个low latency的5T的object存储（？？？）。通过扩展就可以
* 常见： 30天内的数据S3， 30-60存s2-IA，超过60存Glacier， 超过1年删除。

notes：
* 设计的时候local cache要足够保存完整的backup（ 不然的话，从backup恢复本地cache的文件数据就只能从WLAN走S3获取）
