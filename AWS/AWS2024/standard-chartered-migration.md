title: Case study: Standard Chartered core banking migrating to AWS Cloud
date: 2024-02-05 10:37:23
tags:
- AWS
- FSI
- Migration
---


* Strategy : Multi-Cloud ; Core Banking first
* Result High-lights:
  * Improved 10 times performance (10 times transaction per sec)
    * ASG with stateless app layer to allow scaling out
    * Caching to reduce database request
    * Modernized PostGreSQL RDS
      * Read Write Separation to allow separate OLTP and OLAP 
  * Improved Resilliency 
    * 3 AZ deployment 
    * Multi-Region deployment (using DataSync for EFS, using mulit-region Aurora)
  * Security Compliance
    * New feature of Aurora DAS (data activity stream), feed important activity to Splunk for analysis and audit
    * Ability to deploy to 42 markets across multiple countries
* Migration Approach
  * DB2 to Aurora (SCT 1 week, Code change, DMS for data migration)
  * DevOps (Infra as code with Terraform, CICD, Blue/green deployment, Self-service approach)
  * Managing Lantency (choose the co-locate DC with existing on-pre workloads)