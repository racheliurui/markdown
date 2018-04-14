title: AWS - DynamoDB
date: 2018-04-11 11:37:23
tags:
- AWS
- DynamoDB
---


# 038.mp4 039.mp4 -- DynamoDB Overview


## No SQL DB

* Secondary indexes
* Atomic Counters

Terminology,
* Table (like collection in Mongo)
* Table has items ; item has attributes
* Table must have primary key
* Primary Key can be partitioned / sorted
* Per table you can have max 5 global secondary index and max 5 local secondary index
  * Global means accross partition
* Query and Scan
  * Query is recommended , query has to use the Primary Key;
  * Scan will be used to scan the whole table (because no primary key no partition filter)
  * Both by default is eventually consistent. Can request a strong consistent query/scan
* Atomic Counters and Atomic updates
  * https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/WorkingWithItems.html#WorkingWithItems.AtomicCounters
* Provisioned throughput
  * Assign by tables / indexes
  * Buy by write/read capacity units
    * Write Capacity needed for a table: Round(itemsize/chargingStandardSize) * (numberOfWritePerSec)
    * Read Capacity needed for a table: Round(itemsize/chargingStandardSize) * (numberOfReadPerSec/IfNotConsisitentReadThen2)
* Steams: underlying mechanism how it works
* Triggers : linked to Lambda

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


Read / write Capacity Unit definition
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ProvisionedThroughput.html

Table name rule
https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.NamingRulesDataTypes.html#HowItWorks.NamingRules
