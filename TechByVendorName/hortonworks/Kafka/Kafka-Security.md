title: Kafka Security
date: 2017-07-10 09:29:33
tags:
- hortonworks
- kafka
- security
---

# kafka Security

Reference Link

https://www.confluent.io/blog/apache-kafka-security-authorization-authentication-encryption/


>For broker/ZooKeeper communication, we will only require Kerberos authentication as TLS is only supported in ZooKeeper 3.5, which is still at the alpha release stage.

__Note__ current hortonworks zookeeper version in HDF3.0 is ZooKeeper 3.4.6.


```
#!/bin/bash
PASSWORD=test1234
VALIDITY=365
# generate keystore for localhost ; valid for 365 days
keytool -keystore kafka.server.keystore.jks -alias localhost -validity $VALIDITY -genkey
# generate client keystore ; valid for 365 days
keytool -keystore kafka.client.keystore.jks -alias localhost -validity $VALIDITY -genkey

####### generate key store for server and client; valid for 365 days ######

# generate new X509 certificate with cert and keys ; valid for 365 days
openssl req -new -x509 -keyout ca-key -out ca-cert -days $VALIDITY


####### generate x509 CA certificate ; valid for 365 days #####

# generate truststore for server, trust ca-cert as CARoot
keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert
# generate truststore for client, trust ca-cert as CARoot
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert

###### both server and client trust ca-cert by import the ca-cert into truststore #####


# generate a cert-file for server
keytool -keystore kafka.server.keystore.jks -alias localhost -certreq -file cert-file
# using ca-cert and password to sign server's cert-file ; valid for 365 days
openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days $VALIDITY -CAcreateserial -passin pass:$PASSWORD
# import ca-cert as CARoot into server's keystore
keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert
# import ca signed cert-signed into server's keystore
keytool -keystore kafka.server.keystore.jks -alias localhost -import -file cert-signed

###### using ca credential to sign certificate for server; import both ca-cert and ca-signed certificate into server keystore  ######

# generate a cert-file for client
keytool -keystore kafka.client.keystore.jks -alias localhost -certreq -file cert-file
# using ca-cert and password to sign client's cert-file; valid for 365 days
openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days $VALIDITY -CAcreateserial -passin pass:$PASSWORD
# import ca-cert as CARoot into client's keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert
# import ca signed cert-signed into client's keystore
keytool -keystore kafka.client.keystore.jks -alias localhost -import -file cert-signed


###### using ca credential to sign certificate for client; import both ca-cert and ca-signed certificate into server keystore  ######
```
