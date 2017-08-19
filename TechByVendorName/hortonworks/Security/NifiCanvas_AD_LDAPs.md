title:  Nifi UI Authentication integrated with LDAP 
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- nifi
- ldap
---


# Configure Nifi using Ldap to do Authentication to Web UI

## Configure from Ambari Console

Ambari -> Nifi -> Configs -> Advanced nifi-login-identity-providers-env

### using LDAP

* if use LDAP the truststore and keystore can just use server's keystore and truststore with no furthur configuration

```xml
<loginIdentityProviders>
<provider>
<identifier>ldap-provider</identifier>
<class>org.apache.nifi.ldap.LdapProvider</class>
<property name="Authentication Strategy">SIMPLE</property>
<property name="Manager DN">CN=TheUserUsedToConnectToAD,OU=IT Accounts,OU=Iron Ore,DC=hortonworks,DC=net</property>
<property name="Manager Password">PasswordForTheUserUsedToConnectToAD</property>
<property name="TLS - Keystore">/etc/security/certificates/nifi/nifi.server.keystore.jks</property>
<property name="TLS - Keystore Password">KeyStorePass</property>
<property name="TLS - Keystore Type">JKS</property>
<property name="TLS - Truststore">/etc/security/certificates/nifi/nifi.server.truststore.jks</property>
<property name="TLS - Truststore Password">TrustStorePass</property>
<property name="TLS - Truststore Type">JKS</property>
<property name="TLS - Client Auth"></property>
<property name="TLS - Protocol">TLS</property>
<property name="TLS - Shutdown Gracefully"></property>
<property name="Referral Strategy">FOLLOW</property>
<property name="Connect Timeout">10 secs</property>
<property name="Read Timeout">10 secs</property>
<property name="Url">ldap://ldap.ent.hortonworks.net:389</property>
<property name="User Search Base">OU=Iron Ore,DC=hortonworks,DC=net</property>
<property name="User Search Filter">sAMAccountName={0}</property>
<property name="Identity Strategy">USE_USERNAME</property>
<property name="Authentication Expiration">12 hours</property>
</provider>
</loginIdentityProviders>
```

### using LDAPs

* if use LDAPs, we should import AD's certificates into server's truststore

```shell
[root@nifi-server01 ~]# /usr/lib/jvm/java-1.8.0-oracle/bin/keytool -import -file /usr/hdf/current/ranger-usersync/conf/symantec-intermediate-ca.cer -alias symantec-intermediate-ca -keystore /etc/security/certificates/nifi/nifi.server.truststore.jks
[root@nifi-server01 ~]# /usr/lib/jvm/java-1.8.0-oracle/bin/keytool -import -file /usr/hdf/current/ranger-usersync/conf/symantec-root-ca.cer -alias symantec-root-ca -keystore /etc/security/certificates/nifi/nifi.server.truststore.jks
```

```xml
<loginIdentityProviders>
<provider>
<identifier>ldap-provider</identifier>
<class>org.apache.nifi.ldap.LdapProvider</class>
<property name="Authentication Strategy">LDAPS</property>
<property name="Manager DN">CN=TheUserUsedToConnectToAD,OU=IT Accounts,OU=Iron Ore,DC=hortonworks,DC=net</property>
<property name="Manager Password">PasswordForTheUserUsedToConnectToAD</property>
<property name="TLS - Keystore">/etc/security/certificates/nifi/nifi.server.keystore.jks</property>
<property name="TLS - Keystore Password">KeyStorePass</property>
<property name="TLS - Keystore Type">JKS</property>
<property name="TLS - Truststore">/etc/security/certificates/nifi/nifi.server.truststore.jks</property>
<property name="TLS - Truststore Password">TrustStorePass</property>
<property name="TLS - Truststore Type">JKS</property>
<property name="TLS - Client Auth"></property>
<property name="TLS - Protocol">TLS</property>
<property name="TLS - Shutdown Gracefully"></property>
<property name="Referral Strategy">FOLLOW</property>
<property name="Connect Timeout">10 secs</property>
<property name="Read Timeout">10 secs</property>
<property name="Url">ldaps://ldaps.ent.hortonworks.net:636</property>
<property name="User Search Base">OU=Iron Ore,DC=hortonworks,DC=net</property>
<property name="User Search Filter">sAMAccountName={0}</property>
<property name="Identity Strategy">USE_USERNAME</property>
<property name="Authentication Expiration">12 hours</property>
</provider>
</loginIdentityProviders>
```


## Reference Links

https://pierrevillard.com/2017/01/24/integration-of-nifi-with-ldap/
