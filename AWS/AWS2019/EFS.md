title: AWS - EFS
date: 2019-07-22 11:37:23
tags:
- AWS
- EFS
---

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
