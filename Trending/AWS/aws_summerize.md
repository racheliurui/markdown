title: AWS - Summarzie
date: 2018-05-04 09:26:23
tags:
- AWS
---

# Service Saling Summary


|   Service Name | Sacling/Failover capability | Comments |
|-----|-----|----|
| AutoScaling| AZ --Yes ; Region -- No| horizental scaling EC2 in same autoscaling group|
| Elastic Cache| Multi AZ failover; | |
| Route53 | Global Service ||
| CloudFront| Global Service ||
| VPC | Span Multi AZ; Region -- No ||
| RDB | Muti AZ; Multi Region (read replica) ||
# Calcuation

EBS --> GP2 typed SSD  --> flexible IOPS
