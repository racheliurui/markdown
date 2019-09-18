title: AWS - EFS
date: 2019-07-22 11:37:23
tags:
- AWS
- EFS
---

# Reference

> https://youtu.be/4FQvJ2q6_oA

* AWS has 3 main adoption patterns, that can be mapped to 3 storage categories
  * Re-Hosting -- Block Storage
  * Re-Platform -- File Storage -- EFS
  * Re-Architecting  -- Object Storage

## What's new

* EFS only support linux; new FSx for Windows File Server
* FSx for Lustre
* Support Multi-VPC access
* AWS DataSync : initial full copy, and subsequent incremental transfers of changed data to cloud ; Muti thread
* TCO Example, 100G standard storage, 400G Infrequent, around $50/month

## Deep Dive

* Performance mode
   * General Purpose , focus on low latency (max 7k iops/sec) -- Recommend to start with
   * Max I/O, focus on I/O (higher latencies)
* Throughput mode
   * Busting Throughput -- Recommend to start with 
   * Provisioned throughput (you can decrease every 24 hours)
* EFS Infrequent Access (85% cheaper)
   * Auto lifecycle management (any file not being accessed more than 30 days)


### Security Model

Network using ACL; Access using POSIX or IAM; Encrypt ; Compiance with HIPAA etc.

## Use case

* Atlassian - JIRA
* T-Mobile
    * K8S with EFS (Persistent Volumes for 100s of nodes)
    * Cache build dependencies with CICD (Maven dependencies as example)
    * Centralized Repository
    * Tibco EMS HA

## Best Practices

* Throughput
  * Multi-Threads
  * Multi Directories
  * Use large IO (aggregate IO)
* IOPS
  * Multi Threads
  * Multi Directories
* Use Cloudwatch to monitor

# Choose the Right performance with File System (2018)

* After 2018, EFS support provision throughput
* Similar with EBS provision but irrelavant with the size of storage, can be modified using CLI
   *  Auto-provision the throughput is in consideration but not available yet
* Demo
   * using ioping: A tool to monitor I/O latency in real time
   * using nload to monitor network status (because EFS is mounted via network)
   * multi-thread will increase the throughput
   * use aws efs cli to update the throughput limit , then on-flight change happened
* EFS mount helper can help you figure out what configuration you need
* EFS cloudwatch ready to use metrics to help you setup and monitor and tune EFS

Use Scenario: web , CICD , DEV, big data, ML , db backup
Compliant: Healthcare , PCI compliant(payment data) ; at-rest and in-transit security both supported ( no extra cost, but will have performance impact); built in support with KMS and CMK.
Soft Limit with EFS:  1G/Sec in all region (can increase when request)
__EFS FileSnc__:  new feature used to migrate local data into EFS multi-threading with security

Security : KMS CMK



# Reference

>
https://youtu.be/TS1wS_Wb6PA
>
https://github.com/koct9i/ioping

>
https://aws.amazon.com/blogs/aws/efs-file-sync-faster-file-transfer-to-amazon-efs-file-systems/
