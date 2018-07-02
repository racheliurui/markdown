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

## Redshift

* Petabyte level
* PostgreSQL based
* continously backed up to S3 with snapshots (1-35 days)
* quick recovery from snapshots

## EMR (Elastic Map Reduce)

* Fully managed hadoop service
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


# Solution: Pipeline, EMR, Redshift

## Related AWS services

* Collect : AWS direct connect; AWS Import/Export; AWS Kinesis
* Store: S3; DynamoDB; Glacier
* Process & Analysis: EMR; Redshift; EC2

![sample big data solution on AWS](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/023_BigDataSolutionOnAWS.png?raw=true)

## Work through a case

Step 1, sending log file to S3
Step 2, pipeline using pig script to parsing log as CSV
Step 3, pipeline to EMR (AWS hadoop)
Step 4, pipeline to Redshift


* Tips
Make use of json to define, build once, deploy many
Make use of __backfill__ : Backfill is triggerred when you specify a scheduled start time to past. The pipeline might start multi concurrent tasks to try to catch up the data till now.

## User experience - Coursera

* Use __Dataduct__ for concise ETL definition (coursera opensourced tools)
* prgrammically create pipeline
* Extract re-usable steps
* SQL->S3->EMR->S3->Redshift
* Persist redshift log to
  * tracking and generating who is responsible for which schema etc.
  * user knows how fresh the data is.
* Data quality : GIGO (Gabage in gabage out; best practise is the source system to fix the issue)
* Automated QA Checks

Benifit for using data pipeline
* Auto start and stop Resources
* Handles access Management
* integrate with aws services



## Reference

> Big data solution with pipeline, emr, redshift
https://youtu.be/oOIgMSv2rug


# Solution :  big data solution for JustGiving website

## Challenges

* Support various of datasource (api click; log; behaviour data, etc)
* performance
* ease of preparation of data

## Pain: DAG (Directed Acyclic Graph) , like a data flow

Solve the pain:
* Use event-driven and severless pipeline
   * separate storage with compute
   * use message, pub/sub patterns:
      * Pattern 1: when data is ready, send notification to topic and trigger the data loading into redshift
      * Pattern 2: when data is ready, queuing EMR task into SQS, run EMR task from the queue to prcocess the data
      * Pattern 3: data present on Kinesis, processed by EMR, and send notification when finished
      * Pattern 4: S3-> Lambda -> S3 (suitable for small files)
      * Pattern 5: Serveless streamify small file and merge into larger file. S3-> Kinesis-> firehose-> S3
      * Pattern 6: adding S3 to host static website with above analysis result
* support ETL and ELT



## Reference

> Big data solution with : S3, Lambda , Redshift, EMR, Kinesis
https://youtu.be/YGNu6SLCk50
