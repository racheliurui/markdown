title: AWS - Elastic Cache hands on
date: 2018-04-22 11:07:00
tags:
- AWS
- Elastic Cache
---

# 093.mp4 094.mp4 -- Elastic Cache hands on

An important use scenario is to maintain application session state ( sesson replication)

* Create Security Group for Redis service as RedisSG.
  * allow inbound 6379 port from webserverSG
  * allow outbound 6379 to everywhere
* __Cache subnet group__
  * similiar like RDB subnet group.
* Using wizard to create Elastic Cache cluster
  * option: enable replication ; enable multiAZ
  * instance type: cache.t2.micro
  * option: file location in S3 bucket
  * select cache subnet group using newly created; select security group using newly created;
  * optional : maintain window , SNS
* review the result ( 1 node )
