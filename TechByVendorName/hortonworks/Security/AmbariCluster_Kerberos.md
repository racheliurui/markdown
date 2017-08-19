title: Kerberize Ambari cluster
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- SSL
---

# Kerberos


Kafka SASL relying on Kerberized cluster.

## configurations for enable Kerberos via Ambari wizard

| Configuration Name | Value |
| ----- | ----|
| KDC Type | Existing Active Directory|
| KDC hosts| kdcserver1,kdcserver2|
| Realm name | DOMAINNAME.CAPITAL.NET |
| LDAP url | ldaps://ldapserver1.domainname.capital.net:636 |
| Container DN | OU=AmbariCluster, DC=net |
| Domains | DOMAINNAME |
| Kadmin host | kdcserver1 |
| Admin principal | SUPERUSER |
| Admin password | password |


## mandatory configuration for Nifi when Kerberos is enabled

### Specify the kerberos provider

make sure the kerberos-provider details is defined at "Template for login-identity-providers.xml".

```xml
<provider>
        <identifier>kerberos-provider</identifier>
        <class>org.apache.nifi.kerberos.KerberosProvider</class>
        <property name="Default Realm">DOMAINNAME.CAPITAL.NET</property>
        <property name="Authentication Expiration">12 hours</property>
</provider>
```

### Check the user mapping

After kerbereros is enabled, the ldap user name logged in may contains domain like username@domain.com

The name might not match with autorization policy in Ranger.

To solve this, we should config the identity mapping for Nifi.

https://docs.hortonworks.com/HDPDocuments/HDF3/HDF-3.0.0/bk_security/content/identity-mapping.html

So that when user name and password is given from Nifi login form, it will be regrexed and submit partial part to Ranger to authorize.

For example,

```config
nifi.security.identity.mapping.pattern.dn=^CN=(.*?), OU=(.*?)$
nifi.security.identity.mapping.value.dn=$1
nifi.security.identity.mapping.pattern.kerb=^(.*?)@(.*?)$
nifi.security.identity.mapping.value.kerb=$1
```
