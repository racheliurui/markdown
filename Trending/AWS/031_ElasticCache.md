title: AWS - Elastic Cache
date: 2018-04-22 10:19:00
tags:
- AWS
- Elastic Cache
---

# 091.mp4 092.mp4 -- Elastic Cache

* Managed in-memory cache service
* key value stores
* sub-mili sec latency to data
* Redis / Memcached data store options
* Multi-AZ capability

## Memcached store Option

* Free and opensource
* Object max size 1MB
* Total max size 7 TiB
* No persistence ; easy to adding node

## Redis Store option

* Free and open Source
* Object max size 512M
* Total max size 3.5 TiB
* persistence; read replica
* Support Notification from Redis Pub/Sub channel
* Support more data structures including : bitmaps, hyperlogs, geo indexes with radius queries ; and also those supported by Memcached
* Support auto sorting of data
* Support HA and Failover

## Updating cache

* Database triggers or Database via Lambda can be used to trigger update cache
* application can be used to trigger cache updates


## Caching Strategies

* 采用哪种策略取决于需求；策略各有优缺点；策略可以混合使用；实现策略取决于代码
 * Lazy Loading
 * Write Through
 * Adding TTL (memcache support seconds , Redis support both seconds or milliseconds)


Elastic Cache vs Cloud Front
Elastic Cache --- Designed to cache in memory , caching query (dynamic cache)
CloudFront --- designed at Edge location , SDN


A __Node__ is the smallest building block of an ElastiCache deployment.
https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/WhatIs.html

Redis append-only files (AOF) ： Redis一种持久化的方式
Redis Multi-AZ with Auto Failover
