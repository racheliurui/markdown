title: Kafka and Kafka Client communication protected by SSL
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- kafka
- SSL
---

# Enable SSL for Kafka and Kafka Client Communication

## Scripts self-signed certificates and keystores and truststores

```shell
keytool -keystore kafka.server.keystore.jks -alias localhost -validity 365 -genkey -dname "CN=broker, OU=kafka" -keypass SuperTrust11 -storepass SuperTrust11
openssl req -new -x509 -keyout ca-key -out ca-cert -days 365 -passout pass:"SuperTrust11" -subj "/C=AU/ST=WA/L=Perth/O=kafka/CN=broker"

keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass SuperTrust11
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass SuperTrust11

keytool -keystore kafka.server.keystore.jks -alias localhost -certreq -file cert-file
openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days 365 -CAcreateserial -passin pass:SuperTrust11

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass SuperTrust11
keytool -keystore kafka.server.keystore.jks -alias localhost -import -file cert-signed -storepass SuperTrust11

keytool -keystore kafka.client.keystore.jks -alias localhost -validity 365 -genkey -dname "CN=client, OU=kafka" -keypass SuperTrust11 -storepass SuperTrust11


keytool -keystore kafka.client.keystore.jks -alias localhost -certreq -file client-cert-file -storepass SuperTrust11
openssl x509 -req -CA ca-cert -CAkey ca-key -in client-cert-file -out client-cert-signed -days 365 -CAcreateserial -passin pass:SuperTrust11
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass SuperTrust11
keytool -keystore kafka.client.keystore.jks -alias localhost -import -file client-cert-signed -storepass SuperTrust11

mkdir -p /etc/security/certificates/kafka
cp /home/company.net/rachel/cert-kafka/kafka.server.*.jks /etc/security/certificates/kafka
chown -R kafka:kafka /etc/security/certificates/kafka
ls -l /etc/security/certificates/kafka

mkdir -p /etc/security/certificates/kafkaClient
cp /home/company.net/rachel/cert-kafka/kafka.client.*.jks /etc/security/certificates/kafkaClient
chown -R kafka:kafka /etc/security/certificates/kafkaClient
ls -l /etc/security/certificates/kafkaClient


```

Add below configuration to Kafka Config
```config
ssl.keystore.location = /etc/security/certificates/kafka/kafka.server.keystore.jks
ssl.keystore.password = SuperTrust11
ssl.key.password = SuperTrust11
ssl.truststore.location = /etc/security/certificates/kafka/kafka.server.truststore.jks
ssl.truststore.password = SuperTrust11
```


## Test one way SSL (Default)


### Java Client connect to Kafka via SSL


```java
//configure the following three settings for SSL Encryption
       props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SSL");
       props.put(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, "/opt/certificates/kafka/kafka.client.truststore.jks");
       props.put(SslConfigs.SSL_TRUSTSTORE_PASSWORD_CONFIG,  "SuperTrust11");

       // configure the following three settings for SSL Authentication
       props.put(SslConfigs.SSL_KEYSTORE_LOCATION_CONFIG, "/opt/certificates/kafka/kafka.client.keystore.jks");
       props.put(SslConfigs.SSL_KEYSTORE_PASSWORD_CONFIG, "SuperTrust11");
       props.put(SslConfigs.SSL_KEY_PASSWORD_CONFIG, "SuperTrust11");

```

## Command Line Client connect to Kafka server via SSL

https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_security/content/ch_wire-kafka.html

Change the producer.properties and consumer.properties file based on default under /usr/hdf/current/kafka-broker/conf, add below lines to each of the file,

```shell
security.protocol=SSL
ssl.truststore.location=/etc/security/certificates/kafkaClient/kafka.client.truststore.jks
ssl.truststore.password=SuperTrust11

#currently not using keystore,??? how to specify client need authentication
#ssl.keystore.location=/etc/security/certificates/kafkaClient/kafka.client.keystore.jks
#ssl.keystore.password=SuperTrust11
#ssl.key.password=SuperTrust11
```


Then trigger the producer using,
```shell
/usr/hdf/current/kafka-broker/bin/kafka-console-producer.sh --broker-list broker1:6668,broker2:6668 --topic testtopic --producer.config /usr/hdf/current/kafka-broker/conf/producer.properties --security-protocol SSL
```

Trigger the Consumer using,

```shell
/usr/hdf/current/kafka-broker/bin/kafka-console-consumer.sh \
--bootstrap-server broker1:6668,broker2:6668 \
--topic testtopic \
--from-beginning --new-consumer --security-protocol SSL \
--consumer.config /usr/hdf/current/kafka-broker/conf/consumer.properties  
```

## Enable Two way ssl

If we want to enable two way SSL then,
1) "ssl.client.auth=required" should be added to the broker setting.
2) Server should already import client cert (already done when generating the keystore and truststores)


### Connect from command line

Add both keystore and trust store to the producer and consumer property files.

```shell
security.protocol=SSL
ssl.truststore.location=/etc/security/certificates/kafkaClient/kafka.client.truststore.jks
ssl.truststore.password=SuperTrust11

ssl.keystore.location=/etc/security/certificates/kafkaClient/kafka.client.keystore.jks
ssl.keystore.password=SuperTrust11
ssl.key.password=SuperTrust11
```

And used the same command line with one way SSL, we can consume and produce messages without issue.
