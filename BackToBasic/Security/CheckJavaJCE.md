title: Java JCE
date: 2017-08-03 09:27:33
tags:
- basic
- java
- security
---
# Background

http://docs.oracle.com/javase/7/docs/technotes/guides/security/SunProviders.html

http://dino.ciuffetti.info/2016/04/how-to-check-if-jce-unlimited-strength-policy-is-installed/


## Checking JCE using Java command line


Here is the command used to check JCE strength on windows. The output should be >=256
```shell
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('AES'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('DES'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('DESede'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC2'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC4'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RSA'));"
jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC5'));"
```
Example of result on windows where JCE is configured
```shell
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('AES'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('DES'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('DESede'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC2'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC4'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RSA'));"
2147483647
C:\java\jdk1.8.0_91\bin>jrunscript -e "print (javax.crypto.Cipher.getMaxAllowedKeyLength('RC5'));"
2147483647
```

Same command can be used on linux to check. Just make sure the jrunscript is triggerred using the one under the <JRE/JDK_home>bin you want to check.
