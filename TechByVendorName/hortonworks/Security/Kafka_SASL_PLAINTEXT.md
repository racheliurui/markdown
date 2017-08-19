title: Kafka and Kafka Client communication protected by SASL
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- kafka
- SASL
---

# Kafka SASL configurations

## Pre-requirements

Kafka SASL requires the Ambari Cluster to be Kerberized.


## Enable SASL_PLAINTEXT

* add below listeners to kafka listeners list,

  SASL_PLAINTEXT://localhost:6669

* security.inter.broker.protocol=SASL_PLAINTEXT

  The default value is PLAINTEXTSASL (after kerberize wizard), should be changed to SASL_PLAINTEXT

  ???Should we change it to PLAINTEXT for performance?

### Test the SASL_PLAINTEXT

#### Test from commandline

* Turn on Ranger-Kafka Plugin
* Check current user (make sure we are not in sudo command line)

```shell
klist
Ticket cache: FILE:/tmp/krb5cc_239506898_CkQLn0
Default principal: myusername@COMPANY.DOMAIN.COM

Valid starting     Expires            Service principal
17/08/17 09:31:23  17/08/17 19:31:23  krbtgt/COMPANY.DOMAIN.COM@COMPANY.DOMAIN.COM
        renew until 24/08/17 09:31:23
```

* in ranger, check the current user have access to the topic

Permissions list:
Publish, Consume, COnfigure, Describe, Create, Delete, Kafka Admin

* Trigger command line

> specify the protocol using
> use full domain name while list the hosts

```shell
/usr/hdf/current/kafka-broker/bin/kafka-console-producer.sh --broker-list broker1.domain.net:6669,broker2.domain.net:6669 --topic topicname --security-protocol SASL_PLAINTEXT
Msg1
Msg2
```

Correspondingly, consume from command line

```shell
/usr/hdf/current/kafka-broker/bin/kafka-console-consumer.sh \
> --bootstrap-server broker1.domain.net:6669,broker2.domain.net:6669 \
> --topic topicname --from-beginning --security-protocol SASL_PLAINTEXT
Msg1
Msg2
```


#### Test from java client

Using keytab is recommended for PROD env.
In test environment, we copy keytab from linux server to use at client side.

* Use ktutil to check the principal of the keytab
https://docs.oracle.com/cd/E19683-01/806-4078/6jd6cjs1q/index.html

add below configuration to client side jvm
```command
-Djava.security.auth.login.config=/opt/certificates/kafka_SASL/kafka_client_jaas.conf
```

The kafka_client_jaas.conf is like this,

```conf
KafkaClient {
    com.sun.security.auth.module.Krb5LoginModule required
    useKeyTab=true
    storeKey=true
    keyTab="/opt/certificates/kafka_SASL/kafka.service.keytab"
    principal="kafka/broker1.company.domain.com@COMPANY.DOMAIN.COM";
};
```

The /opt/certificates/kafka_SASL/kafka.service.keytab is copied from server which have access to Kafka service. Or, we can generate for certain service account in AD and assign access to the account via Ranger.


And in Java Client code, add below properties. "kafka" is the service name defined in kafka_jaas.conf at /usr/hdf/current/kafka-broker/conf
```java
props.put("security.protocol", "SASL_PLAINTEXT");
props.put("sasl.kerberos.service.name", "kafka");
```



## Back compatible

Once Ranger-kafka plugin is turned on, the PLAINTEXT protocol port will be treated as ANONYMOUS, if we still want PLAINTEXT port to be accesible , we need to allow user ANONYMOUS to have access from Ranger.
> manually add a user called "ANONYMOUS" in Ranger and apply corresponding access to this user in policy.
