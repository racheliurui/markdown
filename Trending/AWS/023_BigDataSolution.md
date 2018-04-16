title: AWS - Big Data Solution
date: 2018-04-20 15:41:23
tags:
- AWS
- Big Data
---

# 069.mp4 -- overview

* Data Storage: Redshift; DynamoDB; S3 ; RDS
* Data Analysis: EMR ; ELK; QuickSight BI; Amazon Machine Learning; Lambda
* Data Streaming: Kinesis Streams

## Readshift

* Petabyte level
* PostgreSQL based
* continously backed up to S3 with snapshots (1-35 days)
* quick recovery from snapshots

## EMR (Elastic Map Reduce)

* fully managed hadoop service
* Clusters can be automatically deleted upon task finish
* Data processing framework: Hadoop Mapreduce & Spark
* Storage options:   HDFS /  EMRFS (S3 based) / EC2 local file system
