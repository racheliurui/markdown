title: AWS - Deployment Service
date: 2018-04-18 13:04:59
tags:
- AWS
- Deployment
---

# 077.mp4 078.mp4 - Deployment Overview

* Infrastructure as code
   * Cloudformation templates;Cloudformation designer;
* Continous Deployment
   * CodeCommit
   * CodePipeline
   * ElasticBeanStalk
   * OpsWorks
   * Elastic Container Service (ECS)
* Application update: Prebaking AMI; in place update application; Disposable upgrade

* Blue-Green upgrade
   * Staged roll out ; need doubled resource; make use of Route53 Service


ELB and ElasticBeanStalk work together
auto create ELB config
https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.managing.elb.html
