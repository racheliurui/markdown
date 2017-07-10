title: Kafka Monitoring
date: 2017-07-10 14:06:33
tags:
- hortonworks
- kafka
- monitoring
---

# Kafka monitoring

The JMX for kafka is by default turned on by Ambari
* default installation will set JMX port at 16667
* default installation with no security turned on

## Check the default JMX monitoring settings

From local machine, switch to the JDK folder and run jconsole

```command
<JDK_Home>/bin/jconsole
```

With the UI, use kafkaBrokerhostName:16667 as the connection string to connect to Kafka.
![jConsole_Kafka]


[jConsole_Kafka]: images/KafkaMonitoring/jConsole_Kafka.PNG
