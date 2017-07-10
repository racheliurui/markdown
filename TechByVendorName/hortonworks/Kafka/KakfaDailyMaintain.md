title: Kafka Common configuration
date: 2017-06-15 16:10:33
tags:
- hortonworks
- kafka
---

# Kafka Common configuration

## Useful Commands

Assuming kafka is installed to default path by Hortonworks Ambari Pack.

* Create a topic
 ```shell
/usr/hdf/current/kafka-broker/bin/kafka-topics.sh --create \
--zookeeper zookeeperserver1:2181,zookeeperserver2:2181,zookeeperserver3:2181 \
--replication-factor 3 \
--partitions 1 \
--topic my-new-topic
```

* List all existing Topics
 ```shell
/usr/hdf/current/kafka-broker/bin/kafka-topics.sh  --list \
--zookeeper zookeeperserver1:2181,zookeeperserver2:2181,zookeeperserver3:2181
```

* Delete a topic
 ```shell
/usr/hdf/current/kafka-broker/bin/kafka-topics.sh  --delete \
--zookeeper zookeeperserver1:2181,zookeeperserver2:2181,zookeeperserver3:2181 \ --topic my-new-topic
```

* Describe a topic
 ```shell
/usr/hdf/current/kafka-broker/bin/kafka-topics.sh \
--zookeeper zookeeperserver1:2181,zookeeperserver2:2181,zookeeperserver3:2181 \
--describe --topic my-new-topic
```

* Publish message to a topic
 ```shell
kafka-console-producer.sh \
--broker-list kafka-broker1:6667,kafka-broker2:6667,kafka-broker3:6667 \
--sync --topic my-new-topic
```

* Consume a topic
 ```shell
/usr/hdf/current/kafka-broker/bin/kafka-console-consumer.sh \
--bootstrap-server kafka-broker1:6667,kafka-broker2:6667,kafka-broker3:6667 \
--topic my-new-topic --from-beginning
```


* Change topic message retention time
 ```shell
 /usr/hdf/current/kafka-broker/bin/kafka-configs.sh \
 --zookeeper zookeeperserver1:2181,zookeeperserver2:2181,zookeeperserver3:2181 \
 --alter --entity-type topics \
 --entity-name my-new-topic \
 --add-config retention.ms=86400000
```
