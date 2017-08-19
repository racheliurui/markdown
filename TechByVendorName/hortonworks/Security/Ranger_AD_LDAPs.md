title:  Ranger Sync user and group information from LDAP to used in Authentication
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- ranger
- ldap
---

# Ranger configuration

## Configure Ranger to sync user/group from LDAP

### connection Parameters

* __LDAP Url__

  * ldaps://ldaps.hortonworks.net:636

* __Binding User (sample of distinguished name)__

  * CN=ADMINUSERTOPULLUSER,OU=IT Accounts,DC=hortonworks,DC=net

* __Parameters for user sync configuration__

 * User Attribute: sAMAccountName
 * User Object Class: person
 * User Search Base: OU=IT Accounts,DC=hortonworks,DC=net
 * User Search Filter: cn=*
 * User Search Scope: sub
 * User Group Name Attribute: memberof


* __Parameters for group sync configuration__

 * Group Member Attribute: member
 * Group Name Attribute: cn
 * Group Object Name: group
 * Group Search Base: DC=hortonworks,DC=net
 * Group Search Filter: cn=*

### Ranger Truststore configuration

As we are using LDAPS, we need to import the AD's certificate into Ranger's Truststore.

Check below configuration from Ambari admin console,
```
ranger.usersync.truststore.file
ranger.usersync.truststore.password
```
Make sure Truststore file exist and password is correct.

If you have existing trust store file, you can import the certification manually if needed.
```shell
[root@rangerServer01 ~]# /usr/lib/jvm/java-1.8.0-oracle/bin/keytool -import -file /usr/hdf/current/ranger-usersync/conf/symantec-intermediate-ca.cer -alias symantec-intermediate-ca -keystore /usr/hdf/current/ranger-usersync/conf/mytruststore.jks
[root@rangerServer01 ~]# /usr/lib/jvm/java-1.8.0-oracle/bin/keytool -import -file /usr/hdf/current/ranger-usersync/conf/symantec-root-ca.cer -alias symantec-root-ca -keystore /usr/hdf/current/ranger-usersync/conf/mytruststore.jks
```

Restart Ranger, and check the ranger user sync log at /var/log/ranger/usersync/usersync.log

Login Ranger to check the users and groups are successfully syncronized.
