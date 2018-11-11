title: AWS - DynamoDB
date: 2018-04-11 11:37:23
tags:
- AWS
- DynamoDB
---

# DynamoDB Deepdive

* RDB optimized for storage; nonsql optimized for compute
* DynamoDB supports both Key-value and document data models

## Terminology

Keys:
  * __Partition Key__; __Hash Key__;
  * __Sort Key__;
  * __Range Key__ ;
     * __Local secondary index (LSI)__
	* __Global secondary index (GSI)__ (max 5 GSI per table)
	* If data size >10G use GSI
     * __Primary Key__ = Partition key + Sort Key
  * Attribute;
  * __composite attributes__ ; __composite key__ : a way of construct partition key

* __Partition Key__ used to decide which partition it belongs to (uidling unordered hash index)
* A Partition has 10G limit, if total storage with same partition key exceeded the limit, then __sort key__ is used.

* Table --> Items --> Attributes --> Partition Keys (Mandatory attribute); Sort Key (Optional)
* DynamoDB each partition will have totally 3 copies (including itself); when write, you will get success response when 2 writes succeed.

* __Hash Range Table__ : a table where Hashkey+RangeKey to identify an item.

## Scaling

* Scaling on __throughput__ : WCU and RCU (CU is Capacity Unit)
  * Partition needed = Roundup((total RCU/3k)+(total WCU/1k))
* Scaling on size (maxsizeperitem=400kb, maxsizeperpartition=10G)
* Final partition = ceiling( ScalingByThrougput, ScalingBySize)
* heat map -- showing by time and partition dimention about which data being requested. If all data access is focused from a specific partition, then we got __Hot Keys__ which we should avoid by re-design the paritition key.
* DynamoDB __Burst capacity__ is built in

# Data Modeling

Store the data how you will access it.

* New feature since 2015: Support documents (JSON)

# Patterns

* Use Lambda as DynamoDB Stored Procedure
* Real Time Voting
   * Write Sharding (Add random key to make sure data is spread into multi partition)
* Event logging -- Don't mix hot data and cold data
   * Time series table (static ttl time stamps)  
      * Archive cold data to S3
      * precreate daily,weekly monthly tables and provision accordingly to current table
   * Time series table (Dynamic ttl time stamps) -- data being updated
      * have a GSIKey to label recent updated data (??? 36min:36s)
	  * Put GSI key as partion key and update timestamp as sort key
	  * using lambda to filter out expired data and rotate into data lake
* Product Catelog - Black Friday
   *  Use cache, and logic of updating cache can be implemented lambda

* Common Pattern - Online Gaming (filter)
   * Problem: filter against non sort key. The engine will read all qualified records
     * Solution 1: create composite key (in mongodb it's called combind index)
	 * Solution 2: __Sparse indexes__
* Messaging App - Mixed small and large attributes --- Wrong
   * large attribute will consume more RCU
   * Solution : split the table. Separate metadata and message body into different table. Provision RCU accordingly.
* Sparse Index -- index created on optional attribute	 
    * for example game-score-table has a column called award. When we want to filter by award, we want a Award GSI being created althrough this field has high chance to be null.	 

## MVCC -- Transaction model in nonsql db

* MultiVersion Concurrency
  * For example, creating partition locks.
    * Manage versioning across items with metadata
	* Tag attributes to maintain multipe versions
	* Code your app to recognize when updates in progress
	* App layer error handling and recovery logic

# New Feature: DynamoDB Streams

* Stream of updates to a table (Asynchronous)
* Exact once ; Strictly ordered (per item)
* 24 hour life time
* Cross-region replication is using this feature
* Even multi-master scenario
* Pattern :use lambda to read the stream and update
* Pattern: use Kinesis to interact with stream and aggregate into multi-target
* Use DynamoDB Stream to update Elastic Cache content
* ListStreams / DescribeStream / GetShradIterator...

# Reference

> Deepdive DynamoDB --- Very good
https://youtu.be/bCW3lhsJKfw

> Deepdive DynamoDB
https://youtu.be/VuKu23oZp9Q


> How to choose the right key
https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/


# 038.mp4 039.mp4 -- DynamoDB Overview


## No SQL DB

* Secondary indexes
* Atomic Counters

Terminology,
* Per table you can have max 5 global secondary index and max 5 local secondary index
  * Global means accross partition
* Query and Scan
  * Query is recommended , query has to use the Primary Key;
  * Scan will be used to scan the whole table (because no primary key no partition filter)
  * Both by default is eventually consistent. Can request a strong consistent query/scan
* Atomic Counters and Conditional Writes
  * https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters


# 040.mp4 -- DynamoDB Handson

* Create a Table
* Select Primary Key & Sort Key ; Index Key & Sort Key
* Add item from Console
* Prepare a JSON file to bulk upload data
* using command line to batch upload data
  ```command
  aws dynamodb batch-write-item --request-items path/to/prepared.json
  ```
* using command line to query the table



Supported Data types
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html


Table name rule
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.NamingRulesDataTypes.html#HowItWorks.NamingRules
charactor, number, underscore, dash, dot
