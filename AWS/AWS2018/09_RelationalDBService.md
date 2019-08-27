title: AWS - Relational Database Service
date: 2018-04-10 15:55:23
tags:
- AWS
- AWS RDS
---

# RDS

## Max Storage limites

* MS SQL Server __4TB__
* My SQL, Oracle, PostgreSQL, MariaDB __6TB__
* Aurora  __64TB__

## Supported Relational DB types

* Support 6 Relational DB types, MS sql;mysql;Oracle; AWS Aurora; PostgreSQL; MariaDB
* Backup - to S3 (can be encrypted for db or snapshot at rest)
* Failover - Multi-AZ ; when master fails , standby is promoted , then CName is updated to poiting to standby, then new instance is created to replace the master
* Read Replica (don't support ms sql and oracle); one DB can have multiple read replicas
   * If you have multiple read replica, then routing from single url using Route53 or customed HAProxy , __not support AWS ELB__

# Security

* Network firewall control
 Like EC2 security group, RDS has its own security group settings
* Access Control
 IAM : IAM can't control who can log in database (database user and groups); IAM only controls who can have what level of access to the RDS service.
* Compliance and Transport SSL
* At Rest Encryption: using KMS(key management service) & Envolope Encryption
  * Limit risk of compromise key
  * Centralized access and audit of key activity
  * just click "enable encryption"
* At Rest Encryption Limitations
  * only available when creation a new database; and once enabled , can't be removed.
  * Unencrpyted snapshot can be changed to encrypted snapshot
  * Encrypted db across region (aws is working on that)

![config to enable encryption](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/09_RDS_EncryptDataAtRest.png?raw=true)


# Metrics and monitoring

* 1 min interval by default
* Enhanced Monitoring -- more detailed , minimum 1 second
* AWS __Performance Insights for RDS__ : designed for RDS, with top SQL and help with identify bottlenecks

# High Availability

* take 30 sec to a few min
* Done automatically by DNS CName binding from Primary to Secondary
* Launch multiple read replica into different regions

* Aurora is different with traditional DB
  * have concept similiar like shards
  * __Read Replica Endpoint__: single point to the back end read replica clusters

# Scaling RDB

* Scale your Master Database
  * by change the instance running the instance, optional "Apply immediately" otherwise wait for maintainance window
  * around 6 min to get it done
  * for database with standby in multiAZ , it will upgrade standby first, flag standby as master and master as standby; update master (around 20min)
  * __automatic scale__: depending on usage, schedule scale up and down

Option1, aws CLI + CRON

```shell
aws rds modify-db-instance --db-instance-identifier sg-cli-test --db-instance-class db.m4.large --apply-immediately
# Scale down at 8PM Friday
0 20 * * 5 ~/scale_down_rds.sh
# Scale up at 4AM on Monday
0 4 * * 1 ~/scale_up_rds.sh
```

Option2, using aws lambda (Python + boto3 lib)

Option3, Metrics + SNS/SQS + Lambda -> scale up / down

* scale number of replicas
* Storage Scaling
* Scale the IOPS provisioned

# Backup & Snapshots

## Backup

Backup is automatic scheduled and configurable

* Aurora
  * Continous , no performance impact
* Other RDB
  * Default retention 1 day , configurable max = 35 days
  * Default daily, can select the backup window
  * for Multi-AZ , will use standby
  * Scheduled the backup at 7PM , then AWS try to get a full copy of data starting from 7PM + every 5 min extra log until the copy is fully finished at 7:30PM. 7:30PM will be the "Latest Restorable Time" and we can go back to any time from 7PM to 7:30PM when we restore a database from this backup.

## Snapshot

Snapshot is manually triggered compared to backup.

## Migration

MySQL Dump --> S3 (multi part upload; snowball )--> restore Aurora from S3

__AWS Database Migration Service__:
database heterogeneous migration

# 037.mp4 -- AWS RDS Hands On


* Create the mysql service --- (PROD must use multi-AZ , but dev can be single instance)
* Change to not use "Generic SSD" which not suitable for DB because of high IOPS
* Enable Multi-AZ
* Publically accesable -- disable; control security using VPC

RDS Security groups
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html

DB Parameter group
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html
数据库的参数组。

DB Option Group ; lifecycle for the option group config (e.g a restored db )
https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithOptionGroups.html
数据库的feature option组。

comments:
Option groups allows the use of available features within a database. So if you spun up a SQL RDS instance, you could configure TDE, Native SQL backup/restore, or mirroring. Parameter groups is how the database is configured. Min/max settings, etc.


# 知识点

* 关于RDS take snapshot时候的I/O suspension
   * 如果是single AZ，数据库肯定有I/O suspension
   * 如果是multi AZ，而且数据库设置了backup， 那么snapshot会在backup数据库上进行，这时候就不会有io suspension

* MySQL engine 的选择

 * InnoDB storage engine ：  Point-In-Time restore and snapshot restore require a recoverable storage engine and are supported for the InnoDB storage engine only.
 * MyISAM storage engine does not support reliable recovery and can result in lost or corrupt data when MySQL is restarted after a recovery, preventing Point-In-Time restore or snapshot restore from working as intended. However, if you still choose to use MyISAM with Amazon RDS, snapshots can be helpful under some conditions.
 * MyISAM performs better than InnoDB if you require intense, full-text search capability.
 * The Federated Storage Engine is currently not supported by Amazon RDS for MySQL.

* 3306是MySQL的默认端口；3389是RemoteDesktop的默认端口



# References

> 035.mp4 036.mp4 -- AWS RDS Services

> Deep Dive
> https://youtu.be/pPLPzPYY5uU

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
