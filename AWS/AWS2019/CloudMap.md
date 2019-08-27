title: AWS - CloudMap
date: 2019-08-15 11:37:23
tags:
- AWS
- CloudMap
---

# Reference

>
https://youtu.be/fMGd9IUaotE


## Service registers

* Zookeeper , Eureka, SmartStack, SkyDns, Doozerd, etcd, etc
* CloudMap : dynamic map of your cloud

# Issue try to solve

* Attribute based service discovery under complex service environment

  * Multiple Stage
  * Multiple Version
  * Multiple Status

* Handle partial failure
  * help you provision Route53 to help handle partial failure

# Integrate with existing AWS service

* Cloudformation
* IAM

## Demo


```
dig +short A backend.cloudmapdemo.com
172.31.1.228
172.31.0.100
```

## Work with Consul

* AWS Cloud Map and Consul to extend hybrid infra to multi-region

>
https://www.youtube.com/watch?v=fMGd9IUaotE&list=PL72BC_ThTrzW0wfjYWsPIG-sRb920Ubs3&index=24&t=614s



## Feeling

* An solution based on Route53.
* A service discovery service.
* provide namespace and service name, it will provide a list of service endpoints.
