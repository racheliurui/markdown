title: AWS - Kinesis
date: 2020-01-18 11:37:23
tags:
- AWS
- Kinesis
---

# Reference 

> 
https://youtu.be/jKPlGznbfZ0

## Why Streaming

* Data loses value quickly over time
    * "Time critical decisions" need streaming data
    * inject as it's generated, process on the fly and do real-time analytics/ML/Alert/Action
* Common streaming use case
    * Smart home / automation / log / Data Lake / IoT
* Real time analytics demo (User Dashboard)

## Streams Producers and Consumers

### Producer limits

* bandwidth limitation: 1MB/sec/shard 
* if not, aggregate your message, and use throughput limitation: 1k record/sec/shard

### Normal consumer 

* The slowest consumer will also impact number of shards, you might need increase the shards to allow the slowest consumer can process the message concurrently to pick up all the messages

* The fastest speed you can get the data is one trasaction per 200ms 
* Multiple consumers share the 5 transaction/sec/shard and 1M data / sec /shard limitations.
  * Multiple consumers will decrease the troughput as well as increase your latency

* Workaround , use master stream and copied slave stream

### Enhanced Fan out
  
* use http/2 , subscribe, and data is pushed to consumer
* each consumer gets dedicated 2MB/sec/shard ; message latency can be 15ms 


## Comcast streaming


* As a platform, design topic for teams to share/communication
* Use API gateway to register the stream


https://comcastsamples.github.io/KinesisShardCalculator/