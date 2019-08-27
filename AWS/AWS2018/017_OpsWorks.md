title: AWS - OpsWorks
date: 2018-04-15 19:31:23
tags:
- AWS
- OpsWorks
---

# OpsWorks Overview

* Use Chef Recipes
* Better and fine-controlled way of define infrastructure (Compare to Elastic Beanstalk)

## CM Model

* CM Model (configuration management)
  * Stack: a set of intances and applications
  * Layers: reusable subcomponent of stack
  * Instances: can participate multiple layer
  * Apps: codes running on server

* Scaling
   * manuall Scaling
   * Automatic scaling : time based; load based
   * can be combined together

* Chef Recipes --- infrastructure as code


# DeepDive

* Difference with Chef Server, (there's no chef server)
  * Can be agentless (push model)


* Push Json format event to define to target status for each of the lifecycle of the server.
  * Setup event
  * Config event
  * Deploy event
  * Undeploy event
  * Shutdown event



# OpsWorks hands on

* Create first stack
* Create sample stack
* Check the git repo for application and git repo for infrastructure
* quicker than ElasticBeanStalk or container --- high end options


# improvements

Separated env for AWS chef recipe and customer recipe to avoid conflicts

First 10,000 metrics	$0.30
$3/dashboard/month
Regular (5 min)	$0.10/alarm
$0.50/GB

# References

> Opsworks 2015 under the hood

> https://youtu.be/WxSu015Zgak

> 054.mp4 055.mp4 056.mp4
