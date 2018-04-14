title: AWS - CLI
date: 2018-04-10 14:00:23
tags:
- AWS
- CLI
---

# 030.mp4 -- CLI


2 ways of interact with AWS via CLI

* install locally, access AWS services via https
* install on EC2, access AWS services via SSH


```aws
aws --version
aws s3 mb s3://newbucket
aws s3 ls
aws s3 rb s3://newbucket
aws ec2 help
aws ec2 describe-instances


aws iam create-user --user-name newuser
```

TODO,
setup on mac and handson aws
