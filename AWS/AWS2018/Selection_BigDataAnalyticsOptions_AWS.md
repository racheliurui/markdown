title: Big Data Analytics Options on AWS
date: 2018-05-19 16:13:23
tags:
- Big Data
- AWS
---

# AWS key advantages (Cloud advantages)

* Fexibility with Failover accross Availability Zones
  * Different data collection options & technique :
    * AWS Data Pipeline , AWS Import/Export Snowball , AWS Mobile Hub, AWS IOT, Kinesis Firehose

# Comparison

## Service Supported Data Volumn and antipatterns

| Service | Data Volumn | AntiPattern |
|--       |          --| ---|
| Kinesis | Terabytes| Steaming throughput <200k/s |
| AWS EMR | N/A | Small data, ACID transaction(RDB) |
| AWS ML | Max 100 G | >100G Dataset(EMR); Unsupported ML Tasks (EMR) |
| DynamoDB | N/A | Join/Complex Transation (RDB), BLOB (S3), Low IO data (S3)|
| AWS Redshift | min 160G - Petabyte | unstructured Data (DynamoDB), OLTP (RDB), BLOB(S3) |
| AWS Elasticsearch Service | 5T | >5T(EMR, EC2), OLTP(RDB) |


## Service Cost model

| Service | Cost Model |
|--       | ---|
| Kinesis | Per Hour Per Shard + 1 million Put Transactions |
| AWS EMR | hourly charge to EMR + EC2 hosting EMR |
| AWS ML | hourly rate to set up model + number of predictions genearted. (Realtime prediction will also charge on memory reserved to run the model) |
| DynamoDB | throughput/hour + data storage/month + data transfer in&out in GB/ month|
| AWS Redshift | node size and number |
| AWS Elasticsearch Service | hourly rate + storage + data transfer |
