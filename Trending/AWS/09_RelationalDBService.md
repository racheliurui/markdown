title: AWS - Relational Database Service
date: 2018-04-10 15:55:23
tags:
- AWS
- AWS RDS
---

# 035.mp4 036.mp4 -- AWS RDS Services

## Supported Relational DB types

* Support 6 Relational DB types, MS sql;mysql;Oracle; AWS Aurora; PostGreSQL; MariaDB
* Backup - to S3 (can be encrypted for db or snapshot at rest)
* Failover - Multi-AZ ; when master fails , standby is promoted , then CName is updated to poiting to standby, then new instance is created to replace the master
* Read Replica (don't support ms sql and oracle); one DB can have multiple read replicas
* Rounting using Route53 or customed HAProxy , not support AWS ELB

# 037.mp4 -- AWS RDS Hands On


* Create the mysql service --- (PROD must use multi-AZ , but dev can be single instance)
* Change to not use "Generic SSD" which not suitable for DB because of high IOPS
* Enable Multi-AZ
* Publically accesable -- disable; control security using VPC

RDS Security groups
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html

DB Parameter group
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html

DB Option Group ; lifecycle for the option group config (e.g a restored db )
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html

comments:
Option groups allows the use of available features within a database. So if you spun up a SQL RDS instance, you could configure TDE, Native SQL backup/restore, or mirroring. Parameter groups is how the database is configured. Min/max settings, etc.

Fee
http://calculator.s3.amazonaws.com/index.html#s=RDS

Best practise
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_BestPractices.html

Restore the RDB
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_PIT.html

RDB maintainance sequence
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.Maintenance.html

Steps to db deletion
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_DeleteInstance.html

Monitoring types
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Events.html


Cloudwatch and CloudTrail to help monitor the db Performance
https://docs.aws.amazon.com/awscloudtrail/latest/userguide/send-cloudtrail-events-to-cloudwatch-logs.html



Automatic Backup; backup retention periods
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html

PIOPS and IOPS and page size ; ratio of the requested IOPS rate to the amount of storage allocated
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_Storage.html


RDS reserved instances
https://aws.amazon.com/blogs/aws/reserved-instance-options-for-amazon-ec2/


DB Subnet group
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#USER_VPC.Subnets
