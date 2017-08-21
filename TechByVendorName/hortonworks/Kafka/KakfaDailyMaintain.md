title: Kafka Env Debugging Tools
date: 2017-07-10 14:06:33
tags:
- hortonworks
- kafka
- command cheetsheet
---


# Kafka Common configuration

## Auto start

For Ambari managed Kafka, configure autostart from Console

http://[ambari-host]:8080/#/main/admin/serviceAutoStart

By default, the Kafka and zookeeper "autostart" function provided by Ambari is stopped. For any env other than __DEV__ this is be modified to enabled.

## Change Kafka configuration

To change kakfa configuration,
* For Ambari managed Kafka Cluster,
  * Modify from Ambari Console
* For manually configured Kafka Cluster
 * modify the kafka.properties file.

### Configurations  

* Allow Topic auto created

 __auto.create.topics.enable__

 Default is true. Should disable in __PROD__ environment

* Allow Topic Deletion

 __delete.topic.enable__

 Default is false. Should disable in __DEV__ environment.

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

# Kafka Monitring Via Ambari

Definition of metrics

https://docs.hortonworks.com/HDPDocuments/Ambari-2.4.1.0/bk_ambari-user-guide/content/grafana_kfka_hosts.html


Bytes In
