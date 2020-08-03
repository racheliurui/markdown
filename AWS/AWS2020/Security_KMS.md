title: AWS - KMS
date: 2020-08-03 11:37:23
tags:
- AWS
- KMS
---


* If you want to use AWS managed keys, then you can't control key rotation, it would be every 3 years.
* If you want to use Customer Managed Keys (CMK), you can turn on automatic rotation for sysmetric keys, it would be every year.
* CMK sysmetric key and asysmetric private key never left KMS unencrypted
* How to choose from Sysmetric and Asysmetric key
> https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-choose.html
