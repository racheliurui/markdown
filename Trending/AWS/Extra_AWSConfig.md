title: AWS - Config
date: 2018-06-29 12:31:00
tags:
- AWS
- AWS Config
---

# AWS Config

Like the Ambari console config change

## Key Features


* Record config -->S3
  * Record metadata;
  * attributes;
  * relationships; ( for example , an EBS is attached to an EC2)
  * current config;
  * related CloudTrail events
* Normalize
* Store
* Deliver: report , snapshot
   * can aggregate to a single S3 from multi accounts across multi Region
   * Can make use of SNS ->SQS to aggregate

# AWS Config Rules

make use of the AWS Config Deliver to set up rules to check config changes ; visualize compliance and identify offending changes

## Trigger by change

* Tag: for example anything with tag "Production" changed then the rule is triggerred
* Resource type
* Resource ID

## Trigger by frequency

# Use Cases

* Security Analysis
* Audit compliance
* Change management
* Trouble shooting
* Discovery

* The aws console will provide muti views. Rule centric and Resource centric.



# Reference

https://youtu.be/sGUQFEZWkho

# Differenciation

https://www.sumologic.com/blog/amazon-web-services/aws-config-vs-cloudtrail/
Both tools are helpful when implementing a self-serve IT policy. Config works well with CloudFormation to enable IT to create approved templates for every type of AWS resource. These templates, when shared with developers, let them provision the resources they require without needing IT’s approval every time. It speeds up development, and importantly, enforces a consistent level of quality across the organization. If an employee changes the template while creating the resource, Config catches the change and notifies IT of the violation. If IT wants to dig deeper, they can use CloudTrail to help discover who made the change, from where, and when.
