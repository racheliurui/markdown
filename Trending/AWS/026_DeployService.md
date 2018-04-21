title: AWS - Deployment Service
date: 2018-04-18 13:04:59
tags:
- AWS
- Deployment
---

# 077.mp4 078.mp4 - Deployment Overview

* infrastructure as code
   * Cloudformation templates;Cloudfrmation designer;
* Continious Deployment
   * CodeCommit
   * CodePipeline
   * ElasticBeanStalk
   * OpsWorks
   * Elastic Container Service (ECS)
* Application update: Prebaking AMI; in place update application; Disposable upgrade

* Blue-Green upgrade
   * Staged roll out ; need doubled resource; make use of Route53 Service


ELB and ElasticBeanStalk work together
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.elb.html
