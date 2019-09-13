title: AWS - Keynotes
date: 2019-08-27 11:37:23
tags:
- AWS
---

# Reference

> https://youtu.be/femopq3JWJg

# Redesign of the DB architecture

## History of Aurora
* Cell based architectures
   * Shared storage
   * Easy plus one failure mode
* The log is the database
   * Across the AZ and shard, it's the log that being moved, not the data.
* Change happens at storage layer, redesign to make the storage layer database awareness.

## History of DynamoDB

* Analysis show that 70% query to relational DB is just key value.
> DYNAMO

* Feature
  * automatic re-sharding
  * DB migration service (From Oracle to Dynamo)

## Basic knowledge with Aurora sharding

* 3 quorums across 3 AZ is not enough, 6 quorums across 3 AZ
  * V=6 (every data have 6 copies all together) means there would be 6 node for the same data, when writing, it needs at least more than 3 nodes being alive.
* When a db using sharding, and has v quorums (servers), we can calculate how many write nodes and read nodes we need by applying the rules.
  * if V=6 (we have a cluster of 6 servers) ; (V/2)=3, Vw>3, so Vw=4 ; 4 nodes will be write consistent; Vw+Vr>V, 4+?>6, so Vr=3

  > Vw + Vr > V
  > Vw>V/2


# Data Lake

* S3 manage 60 terabit /sec in one region
* Culture of durability
* 11 9s : Time to Fail and Time to Repair

# 1 Nov 2018 - World's largest Oracle DW to Redshift

* Redshift concurrency scaling
   * consistently fast with thousands of concurrent queries.

# Demo - Fender Music

# Serverless

* Lambda handles trillions of request per month
* Random spread the work load to multi servers
* Lambda Layers (share binaries between different lambda functions)
* Nested Application with Lambda

# StepFunction

* Services that can be orchistrated by StepFunctions
   * Batch , ECS, Fargate, Glue, DynamoDB, SNS, SQS, SageMaker

# API Gateway

* Websocket support for API Gateway
* Move things from EC2 to serveless without change the API

## Kinesis and Managed Streaming for Kafka

* Video and audio becoming streaming data
* Kinesis Family

# Demo - NAB

* Culture, you build it, you fix it, --- craftsmanship
* 35% application in cloud by 2020
