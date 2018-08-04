title: AWS - Kinesis
date: 2018-08-03 16:03:23
tags:
- AWS
- Kinesis
---

# Kinesis Deepdive

* No 1 popular scenario : moving small and fast moving data into persistent layer
* No 2 popular scenario : Steaming data , NRT notification systems


Kinesis:
* managed services
* streaming data ingestion
* continously processing

Small , fast moving data, being captured quickly , then being consumed concurrently by multi different consumers for different analytics Purpose.
* You can split / merge shards via console

## best practises

### partition key strategy

* Avoid hot shard
  * use random partition key
  * use high cardinality key
  * use business key : per billing customer or per device id or per stock symbol

### provision shards

* provision enough shards
* give some head-room in the event of application failures

### put data into Kinesis

* do micro-batch before put
* consider async producer by AWS SDK
  * Kinesis-Log4j-Appender
* __provisionedThroughputExceeded Error__
  * retry
  * re-shard
  * track & monitor
* command to scale up

```cmd
java -cp KinesisScalingUtils.jar-complete.jar -Dstream-name=myStream -Dscaling-action=scaleUp -Dcount=10 -Dregion=eu-west-1
```

### ingest data from kinesis

* Amazon JDK
  * one worker maps to one shard
  * libary to feed data into S3, DynamoDB , Redshift, Elastic Search.
  * feeding data following below pipeline,
    * ITransformer: transform the data read from Kinesis
    * IFilter: filter only data interested
    * IBuffer: batching the data before sending out (for example to S3 or Redshift, better buffer to MB level before sending out)
  * connector to redshift will put data into S3 first and buffer it then send to redshift
* application consuming the data better has the capability to scale automatically
* use Matric to detect why the consumer is slow
  * GetRecord.Latency
* build flush-to-S3 consumer to capture original data (by number; by byte ;by time)


# References
> https://youtu.be/8u9wIC1xNt8
