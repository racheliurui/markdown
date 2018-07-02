title: AWS - OpsWorks
date: 2018-04-15 19:31:23
tags:
- AWS
- OpsWorks
---


# DeepDive

* Difference with Chef Server
  * Can be agentless (push model)
* Push Json format event to define to target status
  * Setup event 
  * Config event
  * Deploy event
  * Undeploy event
  * Shutdown event

# 054.mp4 055.mp4 --OpsWorks Overview


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

# 056.mp4 -- OpsWorks hands on

* Create first stack
* Create sample stack
* Check the git repo for application and git repo for infrastructure
* quicker than ElasticBeanStalk or container --- high end options
