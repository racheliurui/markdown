title: Ambari SNMP Alert Setting
date: 2017-07-05 10:10:33
tags:
- hortonworks
- ambari
- snmp
---

# Ambari SNMP Alert Setting

# Test Environment

Description


## Prepare the SNMP Test Server

* install snmp
 ```shell
yum install net-snmp net-snmp-utils net-snmp-libs –y
```
* change authorization

 ```
# authCommunity   log,execute,net public
# traphandle SNMPv2-MIB::coldStart    /usr/bin/bin/my_great_script cold
disableAuthorization yes
```

* add Ambari MIB definition
The current version of Ambari (2.4.2) does not contain MIB definition file. Manually copy the content from ambari jira
https://issues.apache.org/jira/secure/attachment/12761892/APACHE-AMBARI-MIB.txt

 ```
vi /usr/share/snmp/mibs/APACHE-AMBARI-MIB.txt
chmod 777 /usr/share/snmp/mibs/APACHE-AMBARI-MIB.txt
```

* Start the SNMP trap daemon to log all traps to /tmp/traps.log for testing purpose
 ```shell
nohup snmptrapd -m ALL -A -n -Lf /tmp/traps.log &
```

* test the Ambari MIB is being respected by SNMP Server

 From the SNMP server, run below command
 ```shell
 snmptrap -v 2c -c public localhost '' APACHE-AMBARI-MIB::apacheAmbariAlert alertDefinitionName s "definitionName" alertDefinitionHash s "definitionHash" alertName s "name" alertText s "text" alertState i 0 alertHost s "host" alertService s "service" alertComponent s "component"
 ```

 Then check the notification/trap is loged in /tmp/traps.log

 ```log
 2017-07-05 12:04:17 UDP: [127.0.0.1]:48795->[127.0.0.1]:162 [UDP: [127.0.0.1]:48795->[127.0.0.1]:162]:
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (379025195) 43 days, 20:50:51.95       SNMPv2-MIB::snmpTrapOID.0 = OID: APACHE-AMBARI-MIB::apacheAmbariAlert   APACHE-AMBARI-MIB::alertDefinitionName = STRING: "definitionName"       APACHE-AMBARI-MIB::alertDefinitionHash = STRING: "definitionHash"       APACHE-AMBARI-MIB::alertName = STRING: "name"   APACHE-AMBARI-MIB::alertText = STRING: "text"   APACHE-AMBARI-MIB::alertState = INTEGER: ok(0)  APACHE-AMBARI-MIB::alertHost = STRING: "host"   APACHE-AMBARI-MIB::alertService = STRING: "service"     APACHE-AMBARI-MIB::alertComponent = STRING: "component"
 ```


## Define notification from Ambari Server

 From Menu __"Alerts->Actions->Manage Notifications"__

 ![DefineAmbariSNMPNotification.PNG]

## Test Ambari Cluster notification can be sent out

 Stop Kafka Broker and check the SNMP Server /tmp/traps.log


 ```log
 2017-07-05 10:47:48 UDP: [10.241.212.35]:39631->[10.241.86.87]:162 [UDP: [10.241.222.35]:39631->[10.241.86.87]:162]:
 SNMPv2-MIB::snmpTrapOID.0 = OID: SNMPv2-SMI::org.5.1.3.1.1232.1 SNMPv2-SMI::org.5.1.3.1.1232.1 = STRING: "

 [Alert] Kafka Broker Process
 [Service] KAFKA
 [Component] KAFKA_BROKER
 [Host] KafkaHost1

 Connection failed: [Errno 111] Connection refused to KafkaHost1:6667
     "   SNMPv2-SMI::org.5.1.3.1.1232.1 = STRING: "
       [CRITICAL] Kafka Broker Process
     "
```




## Reference Url

https://community.hortonworks.com/articles/74370/snmp-alert.html

https://github.com/apache/ambari/tree/trunk/contrib/alert-snmp-mib

<!--images git
[DefineAmbariSNMPNotification.PNG]:images/AmbariSnmpTesting/DefineAmbariSNMPNotification.PNG
-->
