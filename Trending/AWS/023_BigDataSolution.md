title: AWS - Big Data Solution
date: 2018-04-17 15:41:23
tags:
- AWS
- Big Data
---

# 069.mp4 -- overview

* Data Storage: Redshift; DynamoDB; S3 ; RDS
* Data Analysis: EMR ; ElasticSearch; QuickSight BI; Amazon Machine Learning; Lambda
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

## ElasticSearch

* datasource: s3, Kinesis Streams, DynamoDB Streams, Cloudwatch logs, CloudTrail
* Not suitable for Petabyte level storage

## QuickSight

* BI Reporting tools
* SPICE (Super-fast , Parrallel , In-memory, Calculation Engine)

# Amazon Machine Learning

* Predictive Analytics
* Datasource: redshift ; S3;  RDS (MySQL)
* Supported Learning tasks: suspicious transactions; forecast product demands; personize content;predict user activity;analyze social media
* has limitation on data set (not too large)
* EMR to run Spark and MLlib

# Kinesis

* Producer support : http put, sdk, c++ lib, java lib (Kinesis agent)
* Consumer support : Java, nodejs, .net,python, ruby
* "Kinesis Firehose" : streaming data into Kinesis Analytics, S3, redshift, elastic search

Multi-zone Redshift design ; AZ and region consideration
https://aws.amazon.com/blogs/big-data/building-multi-az-or-multi-region-amazon-redshift-clusters/
https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-clusters.html

Redshift Schema
https://docs.aws.amazon.com/redshift/latest/dg/r_Schemas_and_tables.html

Kinesis Pricing : "Shard Hour" and "Put Payload Unit" (and "extended data rentention")
https://aws.amazon.com/kinesis/data-streams/pricing/

Kinesis copy data into multi-zones
https://aws.amazon.com/kinesis/data-streams/faqs/

EMR persist cluster
https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-longrunning-transient.html

EMR Pricing (number of node * number of second (min 1 min))
https://aws.amazon.com/emr/pricing/
