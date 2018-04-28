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
