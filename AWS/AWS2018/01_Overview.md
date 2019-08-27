title: AWS - Overview
date: 2018-02-05 09:13:23
tags:
- AWS
---


# Terminology

* 16 Regions : 不同的Region价格不同，部署的服务不同
* 42 Availability Zone : zone之间的故障是完全隔离的
* 50 Edge Locations: 缓存；加速


# Services

FAAS (function as a service; serverless service)
问题：那些服务是serverless的，哪些不是？ 给定一个场景，需要哪些服务的组合？
https://aws.amazon.com/serverless/
AWS serveless service： lambda, dynamodb, api gateway , S3, AWS Step Functions,SNS,SQS, Kinesis, Athena （interactive query against big data), tools and services (city9)

## Compute services

* EC2
* ECS （docker）
* Elastic BeanStalk： 自动部署环境。
* ELB （balancer）
* Autoscaling
* Lambda


## Storage Services

* S3
* Glacier
* EBS (Elastic Block Storage): attach to EC2 instances
  EBS to EC2 , n:1
* EFS (Elastic File Storage) to EC2, n:n
* Storage Gateway: 用来连接和同步s3 bucket with objects with 企业私有的数据中心。
* Snowball Device：比storage gateway快。

S3在VPC（virtual private cloud）之外。VPC需要创建Endpoint 去连S3 bucket with objects（存储服务的映射），S3 bucket再连Glacier （通过Glacier Vault）， 从而定义archive的规则。

## database

* RDS ：oracle ， sql server， mysql， postgres, Aurora, MariaDB (MySQL community version)
* Aurora： 企业版MySQL/PostgreSQL
* Dynamodb： servless ， nosql
* redshift： datawarehouse （petabytes）， based on postgres
* ElastiCache （两种引擎可选：Redis， memcache）
* AWS database migration service: Oracle -> Aurora PostgreSQL

Database sits in VPC

## network and content delivery

VPC  
Cloud Front: Caching (Edge)
Route53: Domain Name Services
Direct connect: connect private datacenter --> AWS
ELB (also compute):
Best Practise: deploy one VPC into muti hi-Availability zones.

## management tools

* CloudFormation: Infrastruture as a code. (YAML or JSON to define Infrastruture, git control)
* CloudWatch: Monitoring & Alarms & Trigger
* Trusted Adviser: Expert System(scan existing Infrastruture, give advices), security, performance, cost,etc...
* OpsWorks : chief receipt (similar with CloudFormation)
* CloudTrail : security and auditing, monitor all the api calls, including all the management calls ; can be integrated with CloudWatch


## Messaging

* Simple Queue Service (SQS): Serveless service
* Simple Notification Service (SNS)
* Simple Email Service (SES): bulk delivery of Email


Process decoupling example,
请求放sqs，连cloudwatch，请求spike的时候，cloudwatch的alarm触发生成更多EC2实例（autoscaling），cloudwatch连SES通知用户。反之scale down。

## Security & Identitiy & Compliance

* IAM （ Identity & Access Management ）： IAM users， groups， roles
* Directory Service - authentication ： 3rd party integration （oauth2） 例如facebook ， google， AD， ldap
* Certificate Manager - SSL Certificates
* KMS （Encryption Key Management Service）
* WAF （Web Application Firewall）：extra layer

## Analytics

* Elastic Map Reduce （EMR）：hadoop as a service
* ElasticSearch Service
* Kinesis： data streaming
* QuickSight： Data Visualization
* Data Pipeline： Process and move data


# AWS Free Tier

EC2 ： 750 hours totally /month
Storage： 5G

Services： 不过期。只有量的限制。
