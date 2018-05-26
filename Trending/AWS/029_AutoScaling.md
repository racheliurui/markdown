title: AWS - Auto Scaling
date: 2018-04-19 10:22:00
tags:
- AWS
- Auto Scaling
---

# 083.mp4 084.mp4 -- Auto Scaling

* can scale up / down
* only scale horizontal
* can scale accross AZ (can't scale accross region !)
* params: min size; max size; desired capacity (init size)

* "Launch configuration"
  * AMI type; Instance Type; Key pair; Security Groups
  * Optional: spot instance bid pricing

* Autoscaling group
  * unhealth instance will be terminted and replaced
* Scaling plans (?)
* Scaling Policy
   * how to trigger: Alarm + policy to decide how to scale
   * trigger what action:  ChangeInCapacity; ExactCapacity; PercentChangeInCapacity

* Scaling Policy Types:
   * Simple Scaling
   * Step Scaling (new feature): allow small changes,like 20% more capacity when 40%<CPU<70%

AutoScaling Margin
* Happens during rebalancing when it's become unbalanced between HA zones
https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-benefits.html


Scaling options
https://docs.aws.amazon.com/autoscaling/ec2/userguide/scaling_plan.html

Save cost
https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-instance-termination.html

PercentChangeInCapacity
https://docs.aws.amazon.com/autoscaling/ec2/APIReference/API_PutScalingPolicy.html

The order of execution for scheduled actions ; time confliction
https://docs.aws.amazon.com/autoscaling/ec2/userguide/schedule_time.html


AutoScaling Lifecycle Hooks; Action Result: ABANDON / CONTINUE
自动起来的实例，可以给一定时间装软件搞配置，一切ready返回信号（abandon或者continue），如果是continue，这个实例就可以加入集群了。
https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html

health check grace period
autoscaling检查新加入实例的健康状态之前必须等待的时间。（确保新实例完全ready再检查）
https://docs.aws.amazon.com/autoscaling/ec2/userguide/healthcheck.html

AutoScaling Standby State
把怀疑有问题的实例设置成standby state状态（还属于集群的一部分），检查其状态，修好了归队。
https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-enter-exit-standby.html

Merge single zone autoscaling group into multi zone group
可以把一模一样的分布在不同AZ的集群merge到一起
https://docs.aws.amazon.com/autoscaling/ec2/userguide/merge-auto-scaling-groups.html

describe-scaling-activities command
https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-scaling-activities.html

Error Msg: Autoscaling: 《》instance(s) are already running.
说明hit当初设置的集群实例数上限了。
https://docs.aws.amazon.com/autoscaling/ec2/userguide/ts-as-capacity.html
