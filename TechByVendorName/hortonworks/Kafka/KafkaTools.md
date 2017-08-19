title: Kafka Env Debugging Tools
date: 2017-07-10 14:06:33
tags:
- hortonworks
- kafka
- tools
---

```shell
mkdir -p /opt/kafka-tools
cp -R /path/kafka-manager-1.3.3.7 /opt/kafka-tools
nohup /opt/kafka-tools/kafka-manager-1.3.3.7/bin/kafka-manager -Dconfig.file=/opt/kafka-tools/kafka-manager-1.3.3.7/conf/hdf-server.conf -Dhttp.port=8888 >/dev/null 2>&1 &

nohup java -jar /opt/kafka-tools/kafkadrop/kafdrop-2.0.0.jar --zookeeper.connect=hdf-server03:2181,hdf-server04:2181,hdf-server05:2181 --server.port=8889 >/dev/null 2>&1 &
```
