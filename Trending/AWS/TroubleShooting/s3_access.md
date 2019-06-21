title: AWS - S3
date: 2019-06-21 11:53:23
tags:
- AWS
- S3
- troubleshooting
---


# Trouble shooting : Public Object Access Denied


* ACL and Bucket Policy all set Public
* Account and Bucket level allow it to be Public
* Observation: object uploaded from console works, object uploaded from another account failed.

Add below to specify the public access as well as assign the original bucket user to have full control

```command
   --acl public-read 
```


# use aws-js-s3-explorer


https://github.com/awslabs/aws-js-s3-explorer
