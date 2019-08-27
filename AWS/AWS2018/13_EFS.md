title: AWS - EFS
date: 2018-04-12 11:37:23
tags:
- AWS
- EFS
---

# 045.mp4 -- Elastic File System : is a NAS


* EFS a Network Attached Storage
 * EFS is a NAS (Network Attached Storage; a File system) ; S3 & Glacier is a webstore  ; EBS is a block
* EFS can be shared by multi EC2 instances
* EFS can grow and can shink ; Throughput scales automatically
* Pay as you go (no minimum)
* As a NAS, it support thousands of connections
* Multi AZ replication


After being mounted as Mount Target, it will be shown as a network storage resource.

## Security Control

# 046.mp4 -- Handson on EFS

* mount EFS into certain subnet in VPC (has to be a subnet)
   * Create EFS
   * Launch an EC2 instance
   * Security setting


## From Q&A

* To load data from on-premise to EFS, you can use SCP (Secure Copy)
* For old classic EC2 not inside VPC, you can use ClassicLink to connect to EFS
* On-premise server can connect to EFS via AWS Direct Connect
