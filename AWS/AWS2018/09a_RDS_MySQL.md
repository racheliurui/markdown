title: AWS - RDS MySQL
date: 2018-08-24 15:13:23
tags:
- AWS
- AWS RDS
- MySQL
---

# Building Your Application with an Amazon Aurora Database (DEM113)

https://youtu.be/-ychuATbqPY

## Key New Feature

* Serverless: Auto provision the computing power you need; scale up and down automatically.
* Aurora parallel query
    *  An option when provision your DB, suitable for DB used for both transaction and analysis
    *  https://aws.amazon.com/blogs/aws/new-parallel-query-for-amazon-aurora/
    *  No extra cost but will be more expensive on IO
* Enable Backtrack (select the backup window)
     * Be able to backtrack , extra cost 10USD/month
* Performance Insight
      * by SQL by user(session)


#  Running a High-Performance Kubernetes Cluster with Amazon EKS (CON318-R1)

https://youtu.be/YQWt6wdAZMU

## Optimize pod placement

1) limit the resource
2) Density vs. Size of pods
3) Anti-affinity : keep the CPU heavy pods onto different hosts

## Use diagram to balance the design

1. Anti-affinity
2. Secretes
3. Number of Nodes
4. Active Namespaces
5. Pod Churn
6. Pod Density
7. Networking

## Use K8S with Database

When choosing the persistence layer you have 3 options, inside pod, outside but in same box, outside box.

37:12

# Data migration

* take backup from replica or slave
* compress backup for transfer
* use primary key sort order where possible
* to speed up data loading : more memory + IOPS
* disable binary logging and
* change some of the configuration to reduce server writing logs to disk (because we are dumping the data, no issue of in-flight transaction)

## terminology

binlog : transaction logs
https://www.cnblogs.com/Cherie/p/3309503.html

Default is 0, V5.6 changed to 1, but not much impact the performance

## Data loading format

SQL:
  * easy and simple
  * for small db
Flatfiles:
  * schema load
  * fault torlerance  (each file loading is a separate transaction)

# Normal steps to migrate database

## From on-premise to RDS

* configure replication target and start replication
* stop the application binding with origin source, stop replication after new target catches up
* promote new target instance
* change app binding pointing to new.

## From RDS to on-premise

* RDS provide Point in time recovery

## RDS data to redshift

* change the binlog config to "ROW"

# Multi-AZ Fail Over

Around 1 min for fail over

* 25 sec -- detect failure
* 5 sec -- promote standby
* 30 sec -- CN Name (DNS) update
* standby sits in different AZ, read replica sits in different region

# important scaling archi

(???)
* for reading intensive application (for example 90% reads) --- create more more read replica
* for writes intensive (for example 20%) ---- 2 SCALE

# Rebooting performance

If mysql is using InnoDB as engine, when rebooting, you can do cache warming to improve the performance. The feature is called CacheWarmer Turned down (cache before turning down)

# Handle Schema Change

* Option 1, promote standby approach
* Option 2, use MySQL 5.6 new feature
   * no blocked DML in most cases
   * Perfomance impact: data reorg(sometimes), cpu io , replica lag
   * 45 min
* pt-online-schema-change tool
  * Less performance impact , but longer (2 hours)
  * needs to start a EC2 and install the tools


# Burst mode

GP2 is designed to burst iops
T2 is designed to burst CPU

* The newer instance types with burst feature can save costs

# Reference

https://youtu.be/ZQnzjhnDloM


# Difference between MySQL and MariaDB

>
https://blog.panoply.io/a-comparative-vmariadb-vs-mysql
