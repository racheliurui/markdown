title: AWS - AWS EMR best practise
date: 2018-05-06 19:15:23
tags:
- AWS Best practise
- AWS EMR (Elastic MapReduce)
---

# Introduction

Moving data to AWS --> Data Collection --> Data Aggregation --> Data Processing --> Cost and Performance Optimizations

## 1. Moving data to AWS

Means moving bulk of the existing data to AWS

* Moving Direction
  * Local Storage <--> AWS S3
    * Local HDFS --> S3
       * using S3DistCp: an extension of DistCp with optimizatios to AWS
       * using DistCp
    * Local Filesystem --> AWS S3
       * opensourced tools support multi-threading: Jets3t / GNU Parallel
       * Aspera Direct-to-S3: a file transfer protocol based on UDP and with optimizations to AWS
       * Device based import/export
       * AWS Direct Connect
         * One-time direct connection: once bulk data trasferred, stop the direct connection
         * On going direct connection: always connected
    * S3 --> Local HDFS
      * using S3DistCP or DistCP
  * AWS S3 --> AWS EMR
  * AWS S3 --> HDFS
* With good optimization: several Terabytes a day

## 2. Data Collection

Means streaming data to AWS

* Apache Flume : collected data can be sent to S3, HDFS and more
* Fluentd: collected data can be sent to S3, SQS , MongoDB, Redis and more


## 3. Data Aggregation

Means, aggregated collected data at proper size before sending to target storage (S3, HDFS, EMR).

* Benefit of Aggregation
  * aggregated files means less uploading times
  * aggregated files give better performance to Hadoop
  * aggregated files have better compression ratio

* How to Aggreate
  * Apache Flume and Fluentd have parameters to support
  * S3DistCp has aggregation feature

* How to decide best aggregation size
  * Background knowledge: how file is splitted
    * If file is saved on HDFS, Hadoop will split the file into multiple data blocks and assign map task to each block
    * If file is saved in S3, EMR will use multiple get to get multiple data blocks of same file from S3 storage (default is about 64MB a block); but file size bigger than 4GB in GZIP format will result in logjams for EMR.
    * If the file is zipped , and with a format not supporting split (GZIP not supporting file splitting), then only 1 mapper task will be assigned
  * Best practise 1: select correct aggregated file size
    * GZIP 1-2GB
    * LZO (other format that support split): 2-4GB
  * Best Practise 2: control the file size at best size
    * it's hard because most of the data collector only support time based file spliting. You might need to adjust the time to make sure the generated file fits in best size.
  * Best Practise 3: select data compression algorithms
    * If you choose GZIP format, then file should be around 1G, otherwise change to algorithms that support splitting
    * Why compression is good: less storage/IO/bandwidth
    * places that we can apply compression in MapReduce
      * input file
      * mapper or reducer's intermediate output: reduce the copy and reduce the data spill
    * Summary, 4 considerations when choosing algorithm
      * speed requirement for compress / decompress
      * data storage size
      * original aggregated file size (need split or not), if it's too big , then have to select an algorithm that support splitting    
      * native lib like gzip usually runs faster
  * Best Practise 4: Data Partitioning
      * here means the organization of the data file should match with the way how you want to process it. For example, data analysised by date better been stored by date (different date, different directory)


## 4. Data processing with AWS EMR

EMR will use HDFS or S3 to do mapreduce.

### picking correct instance

* Memory intensive job choose m prefix EC2 family
* CPU intensive job choose C prefix EC2 family

### picking right number of Instances

* If you want it faster, then make sure each split has a mapper task running in parallel, and depending on EC2 instance type, for example m1.small can run 2 mappers in parallel (m1.large can run 30 mappers), you can calculate the number of instances needed.
* If you don't care the speed, then just choose less number of instance, the tasks will be queued
* EMR will charge by full hour.

### estimating number of mappers

* given the block size of S3 or HDFS, we know how a given file is splitted. Then we can calculate the mapper number needed.
* Or the JobTracker GUI or output will tell

### picking EMR type

* Transient EMR Cluster
  * Suitable for data in S3; Processig is not all day long continious ; job is intensive ,iterative data processing
  * For machine learning, data load once , calculated multiple times.
* Persistent EMR Cluster

### Common EMR patterns

* Data sits in S3 and EMR do not have local HDFS, it directly pull data from S3 and do map reduce
* Data sits in S3 and EMR will copy to local HDFS and then do map reduce ---> Suitable for Machine Learning, copy once, calculate many times
* Data sits in HDFS and backup & result to S3 --> suitable when data loss is acceptable
* Manual Adjust the cluster by monitor: number of mappers running; number of mappers outstanding; number of reducers running; number of reducers outstanding;
* Dynamic adjust the Cluster
  * Master Node (JobTracker and NameNode); Core node(TaskTracker and DataNode); Task node (TaskTracker)
  * task node which do no contains HDFS storage can be add and removed dynamically
  * CloudWatch metrices can be used to decide how to adjust the cluster dynamically

## 5. Optimizing Cost

* <17% planned use on-demand
* >17% planned use reserved
  * light reserved for planned a few hours' job
  * medium reserved
  * heavy reserved for all day round job
* unplanned short period and can wait : on spot instance
  * the more the bit close to on-demand price, the more chance to get it done
  * If data is not in S3, then master node shouldn't put on spot instance; core node better not put on spot instance ( at least put more in on-demand or reserved nodes).

# 6. Performance Optimization

* data structure is key for Performance
* hadoop is batch-processing measured by hours to days, if you need improve time by a few minutes to meet SLA, then look at Storm or Spark
* EMR charges on hourly increments.
* make use of task node

## benchmark testing

* eliminate variables while testing. For example, load data to HDFS if you are testing CPU and Memory, otherwise dataloading time from S3 might be the bottleneck.
* Make use of Ganalia to monitor your EMR.

Refer the original document for other Performance Test tips
