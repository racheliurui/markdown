title: keytool
date: 2017-08-09 16:10:33
tags:
- linux
- keytool
- security
---


## p12


* list p12 contents then extract certificate from p12

```shell
keytool -v -list -storetype pkcs12 -keystore KEYSTORE_ABSOLUTE_PATH.p12
keytool -exportcert -keystore KEYSTORE_ABSOLUTE_PATH.p12 -storetype PKCS12 -storepass KEYSTORE_PASSWORD -alias ALIAS -file EXPORTED_CERT_NAME.crt
```
