title: AWS - Database family
date: 2019-07-22 11:37:23
tags:
- AWS
- Differentiation
---


# Use the right Tool for the right job

Aurora benefit :
* 5x throughput vs MySQL and 3x to postgres
* Max 15 read replica
* six copies of data across 3 AZ and continuous backup to S3

* AWS DMS (Data Migration Service)


# New Tools

> Data tools are not competing  each other, they are complementing each other.
> Pick the use case then apply the corresponding tech

* RDB
* Key-value
* Document
* In-memory
* Graph (Nepture)
* Time-Series
* Ledge

## RDB Key-value Graph

RDB: data integrity ; transaction
Key-value: partitioned by keys, consistent performance at scale
Graph: __Vertices__ and Edges

## Case Study

* Airbnb
   * Dynamo for use search history
   * ElastiCache : caching
   * RDS : transaction data

* A book store
  * Used DynamoDB (key-value) to put book information
  * ElastiSearch --- Steam dynamodb change to trigger lambda to put into elastisearch index
  * leader board --- use elasticache ; (???) sorting
  * Recommendation engine -- use graph db to record people with book and purchases

## Ledger Database

Industry: Healthcare, Government, Manufactures, HR&Payroll

* I want the data to be immutable, can be tracked back, can be Cryptographically Verifiable
* Blockchain is hard to maintain

# Reference

> Databases on AWS: The Right Tool for the Right Job
> https://youtu.be/-pb-DkD6cWg
