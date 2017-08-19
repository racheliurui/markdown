title: enable SSL for Ranger Web UI
date: 2017-07-10 14:06:33
tags:
- hortonworks
- security
- ranger
- SSL
---
Turn on Ranger Admin UI HTTPs using self-signed certificate

https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.1/bk_security/content/configure_ambari_ranger_ssl_self_signed_cert_admin.html
```shell
[root@ranger-server01 conf]# keytool -genkey -keyalg RSA -alias rangeradmin -keystore ranger-admin-keystore.jks -storepass xasecure -validity 360 -keysize 2048
What is your first and last name?
  [Unknown]:  ranger-server01.hortonworks.net
What is the name of your organizational unit?
  [Unknown]:  IT
What is the name of your organization?
  [Unknown]:  HORTONWORKS
What is the name of your City or Locality?
  [Unknown]:  Perth
What is the name of your State or Province?
  [Unknown]:  WA
What is the two-letter country code for this unit?
  [Unknown]:  61
Is CN=ranger-server01.hortonworks.net, OU=IT, O=HORTONWORKS, L=Perth, ST=WA, C=61 correct?
  [no]:  yes

Enter key password for <rangeradmin>
        (RETURN if same as keystore password):

```


## Reset the environment for Ranger

If ranger configuration is wrong, we can
1) backup the ranger database
2) stop ranger
3) drop the database, recreate, give corresponding PRIVILEGES
4) start ranger
The table will be automatically recreated and configured.
