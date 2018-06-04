title: AWS - Elastic Cache
date: 2018-04-22 10:19:00
tags:
- AWS
- Elastic Cache
---

# Elastic Cache

* Managed in-memory cache service
* key value stores
* sub-mili sec latency to data
* Redis / Memcached data store options
* Multi-AZ capability
* increase application throughput : 20M reads /sec ,4.8M write /sec
* Scaling DB layer is much more expensive compared to scaling caching layer

## Compare the two options

* depending on project language & framework support
* Redis feature is superset of Memcached

## Memcached (mem cache d) store Option

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
* Support more data structures including : bitmaps, hyperlogs,__GeoSpacial__ command;  geo indexes with radius queries ; and also those supported by Memcached
* Support auto sorting of data
* Support HA and Failover
   * Failover is automatic, will chose the read replica with lowest latency and will switch the dns automatically
* API provided to query all read replica endpoints
* Sharding : 16384 sharding slots (automatic client sharding; developer must use Redis Cluster Client )

### Redis key feature

* Set : A group of objects with key. Each value has a unique key which will be unique.
* Sorted Set: based on set but the keys are sorted
* List: Push Pop ; like queue
* Hashes: key

## Updating cache

* Database triggers or Database via Lambda can be used to trigger update cache
* application can be used to trigger cache updates


## Caching Strategies

* Lazy Loading: load from cache if missed, load from DB and update cache
* Write Through: write DB and then write cache
* Adding TTL (memcache support seconds , Redis support both seconds or milliseconds)


Elastic Cache vs Cloud Front
Elastic Cache --- Designed to cache in memory , caching query (dynamic cache)
CloudFront --- designed at Edge location , SDN


A __Node__ is the smallest building block of an ElastiCache deployment.
https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/WhatIs.html

Redis append-only files (AOF) ： Redis一种持久化的方式
Redis Multi-AZ with Auto Failover

# Use Scenarios

* Session Replication
* IoT Device Data: make use of the 4.x million write / second capability
![IoT with Elastic Cache](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/031_ElasticCache_IoT.png?raw=true)

![IoT with Elastic Cache - another use case ](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/031_ElasticCache_IoT2.png?raw=true)

![IoT with Elastic Cache -  use case 3 ](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/031_ElasticCache_IoT3.png?raw=true)

![IoT with Elastic Cache -  use case 4 ](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/031_ElasticCache_IoT4.png?raw=true)

* GEO advertising (Redis support GeoSpacial )

# Hands on

```
https://github.com/mikelabib/elasticache-memcached-php-demo

Step 1: Install php, apache, memcache client on the server.
e.g. yum install php httpd php-pecl-memcache

Step 2: Update /etc/php.ini file params:
session.save_handler = memcache session.save_path = "tcp://elasticache-memcache-node1-endpoint:11211,tcp://elasticache-memcache-node2-endpoint:11211, etc."

Step 3: Configure php.d/memcache.ini param values:
memcache.hash_strategy = consistent memcache.allow_failover = 1 memcache.session_redundancy = 3

Step 4: Restart httpd
e.g. etc/init.d/httpd restart

```

# Cost

* CrossAZ data transfer is free

# Reference

> https://youtu.be/e9sN15a7utI
