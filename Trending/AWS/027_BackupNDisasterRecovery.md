title: AWS - Backup and Disaster Recovery
date: 2018-04-18 13:25:59
tags:
- AWS
- Backup and Disaster Recovery
---

# AWS Disaster Recovery

http://www.ecloudgate.com/Doc/DisasterRecovery_Overview

Backup and Restore vs Pilot Light vs Warm Standby
cheap ---> Inexpensive
Slow ----> quick
RPO high --> Low which mean time to recover from high to low
RTO high --> Low  which mean data loss time period from high to low

# 079.mp4 080.mp4 - Backup and disaster recovery

## RPO and PTO
两个用来定义灾备需求的重要参数。用于指导灾备技术的选择。

RPO: Recovery Point Objective. The age of files that must be recovered from backup storage for normal operations to resume if a computer, system, or network goes down as a result of a hardware, program, or communications failure.
The recovery time objective (RTO) is the maximum tolerable length of time that a computer, system, network, or application can be down after a failure or disaster occurs.
https://whatis.techtarget.com/definition/recovery-point-objective-RPO
https://whatis.techtarget.com/definition/recovery-time-objective-RTO

## Full backup vs Incremental Backup

* Full backup, good RTO, bad RPO
* Incremental backup , Good RPO, slower RTO

## Redundant Array of Inexpensive Disks (RAID)

* Deinition of Raid0 , raid 1


## recovery by service

### EC2 recovery

* EC2 Image --> S3
* EBS -> Snapshot incrementally to S3
* Setup multiple EBS volumn as RAID1
* EBS is automatically replicated within AZ

### RDS Recovery

* Automatically : daily and retention 1 day (default) max 5 days; RPO is 5 min(??)
* Manually Triggered: won't be delete upon db deletion


### DynamoDB

* no automatic backup
* Via table export -> s3. Can configure daily export and can configure how many percent of throughput used as backup.

## S3

* to Glacier
* Restore from Glacier need 3-5 hours

## Storage Gateway

Hybrid Storage Solution

* File Gateway : S3, EFS
* Volumn Gateway: iSCSI to S3, EBS
* Tape Gateway : iSCSI Virtual Tape Libraries (VTL) to S3, Glacier/Tapes


EBS replication and Raid (what's the relationship, do we still need RAID if it's already being replicated?)
https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/raid-config.html
