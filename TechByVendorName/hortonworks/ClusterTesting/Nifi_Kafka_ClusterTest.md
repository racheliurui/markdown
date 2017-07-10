title: Nifi Kafka Cluster Test Cases
date: 2017-06-15 16:10:33
tags:
- hortonworks
- nifi
- kafka
- cluster
- message sequencing
---

# Nifi Kafka Cluster Test Cases
## Topology

Due to environment limitation, the test environment are two Linux servers creating an Ambari cluster with,
* A zookeeper cluster shared by nifi and Kafka cluster,
* A nifi cluster with two nodes,
* A Kafka cluster

## Message Sequencing Tests

### Message Sequencing Test -- topic without partition
#### Test0, Basic scenario, Subscribe & Process Kafka messages from Nifi Cluster (Single Threading)
![image](https://user-images.githubusercontent.com/5424421/27468239-4aae6112-581b-11e7-9dd2-7ecdd11b378c.png)
* Pre-Step, put sequentially 10000 messages in TopicA and 10000 messages in TopicB
    * both topic with 2 replicas, no partition
    * messages are putting into the queue with sequence, (like Msg1, Msg2...)
* Test Step, Start Nifi Flows
    * one flow subscribe TopicA and put subscribed message into TopicA.D
    * one flow subscribe TopicB and put subscribed message into TopicB.D
    * all nifi processors running on Nifi cluster and configured as __"Concurrent Tasks =1" and Execution = "Primary node"__
![image](https://user-images.githubusercontent.com/5424421/27468280-90249414-581b-11e7-8193-aa19336f44c1.png)
    * subscribe processor is set to consume from "earliest" and with unique client Id.
* After the step, read messages from TopicA.D and TopicB.D, verify that messages are in sequence.

__Conclusion__: messages still remained the sequence, although the Nifi have 2 instances.

#### Test 1, huge volume (50k) of messages passing through
Similar as above, but change the volume of the message to 50k, passed.

#### Test 2, Subscribe & Process Kafka messages from Nifi Cluster (multi-thread allowed)
* Pre-Step, put sequentially 20000 messages in TopicA
    * topic with 2 replicas, no partition
    * messages are putting into the queue with sequence, (like Msg1, Msg2...)
* Test Step, Start Nifi Flows
    * the flow subscribe TopicA and put subscribed message into TopicA.D
    * all nifi processors running on Nifi cluster and configured as __"Concurrent Tasks =1" and Execution = "All nodes"__
    * subscribe processor is set to consume from "earliest" and with unique client Id.
* After the step, read messages from TopicA.D, verify that messages are in sequence.

__Conclusion__: messages still remained the sequence, although the Nifi have 2 instances and processors are allowed to run on both nodes.

#### Test 3, Subscribe & Process Kafka messages from Nifi Cluster (failover)
* Pre-Step, put sequentially 40000 messages in TopicA
    * topic with 2 replicas, no partition
    * messages are putting into the queue with sequence, (like Msg1, Msg2...)
    * check Nifi Primary Node host name and instance process id
* Test Step, Start Nifi Flows, __in middle of the processing kill the primary nifi instance__
    * the flow subscribe TopicA and put subscribed message into TopicA.D
    * all nifi processors running on Nifi cluster and configured as __"Concurrent Tasks =1" and Execution = "Primary nodes"__
    * subscribe processor is set to consume from "earliest" and with unique client Id.
* After the step, read messages from TopicA.D, although the nifi instance is killed in the middle of message processing, the message the message sequence.
#### Test 3 retest, increase message volume to 50k (failover)
have an issue with message processing, but after changing the message publisher to correct version (Kafka 0.10), problem solved.

__Conclusion__: messages still remained the sequence, batching & transaction works to maintain message sequence.

### Message Sequencing Tests Series  --- Kafka Topics With Partition

```sh
$ kafka-topics.sh --create --zookeeper {zServer1:2181,zServer2:2181} --replication-factor 2 --partitions 2 --topic TopicA
```

![image](https://user-images.githubusercontent.com/5424421/27468308-bb0251e4-581b-11e7-8ce0-9f96ce0e4759.png)

Assign different keys while sending out the message, and check the behaviour of partition assignment.


```java

producer.send(new ProducerRecord<String, String>(topic, "Avasdr", "Avasdr.Msg" + i), new Callback() {
                public void onCompletion(RecordMetadata metadata, Exception e) {
                    if (e != null) {
                        e.printStackTrace();
                    }
                    System.out.println("key: " + "Avasdr" + ", Partition: " + metadata.partition());
                }
            });
```

__Conclusion__ the message is sending out to different partitions using round-robin (with a group of 3) strategy.

* For example, messages with 3 different keys will be sent all to partition 0 ; messages with 5 different keys, the first 3 keys will be assigned to partition 0, and rest 2 keys to partition 1.

Here's an example of consuming message from specified partition from Java client,
```java
TopicPartition partition = new TopicPartition(topicName, PartionNum);
consumer.assign(Arrays.asList(partition));
for(int i=0;i< numOfMsg;i++) {
    ConsumerRecords<String, String> records = consumer.poll(batch);            
    for (ConsumerRecord<String, String> record : records) {
        System.out.println("message value," + record.value());
    }
	}

```

## Message Sequencing Conclusion
The easiest way of maintaining message sequence as well as have the capability to parallel processing is to publish messages to Kafka Topic with the key.
Messages with the same key will always be sent to the same partition and one partition is guaranteed to be consumed by only one consumer from Nifi-Kafka Client (even if we allow muti-threading).

## Reference Documents
 * https://www.confluent.io/blog/how-to-choose-the-number-of-topicspartitions-in-a-kafka-cluster/
 * https://howtoprogram.xyz/2016/06/04/write-apache-kafka-custom-partitioner/

> Although it’s possible to increase the number of partitions over time, one has to be careful if messages are produced with keys. When publishing a keyed message, Kafka deterministically maps the message to a partition based on the hash of the key. This provides a guarantee that messages with the same key are always routed to the same partition. This guarantee can be important for certain applications since messages within a partition are always delivered in order to the consumer. If the number of partitions changes, such a guarantee may no longer hold. To avoid this situation, a common practice is to over-partition a bit.

## Site-to-site Protocol

__Test Steps__

* prepare 50k messages in TopicA
* subscribe messages from Nifi Cluster (concurrency=1, run on primary node), and then send to remote nifi via site-to-site protocol
* remote nifi read the message and publish to TopicA.D
* verify message sequence

 __Configuration__
* All kafka processors are using version 0.10
* All publisher processors are setting to "Guarantee Replicated Delivery"
* Set single threading and "FIFO"

### Site-to-site push

### Site-to-site push via HTTP

### Site-to-site push via RAW

Passed.

![image](https://user-images.githubusercontent.com/5424421/27573921-ba90aa02-5b46-11e7-806f-cea9802621cb.png)
### Site-to-site push via RAW (failover)
50k message
kill the primary nifi in middle of processing. Failed.
Result:   target topic get 50k messages. But sequence is wrong. Around 8k messages are received after Nifi instance started again.
![image](https://user-images.githubusercontent.com/5424421/27670978-452cf0bc-5cc3-11e7-89df-f412b1da2c41.png)
### Site-to-site pull
### Site-to-site pull via HTTP
Passed.
![image](https://user-images.githubusercontent.com/5424421/27573894-a0af1484-5b46-11e7-83ee-5ff2bf5d4951.png)
### Site-to-site pull via RAW
