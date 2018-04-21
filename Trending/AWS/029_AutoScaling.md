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
https://docs.aws.amazon.com/autoscaling/ec2/userguide/lifecycle-hooks.html

health check grace period
https://docs.aws.amazon.com/autoscaling/ec2/userguide/healthcheck.html

AutoScaling Standby State
https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-enter-exit-standby.html

Merge single zone autoscaling group into multi zone group
https://docs.aws.amazon.com/autoscaling/ec2/userguide/merge-auto-scaling-groups.html

describe-scaling-activities command
https://docs.aws.amazon.com/cli/latest/reference/autoscaling/describe-scaling-activities.html

Error Msg: Autoscaling: instance(s) are already running. 
https://docs.aws.amazon.com/autoscaling/ec2/userguide/ts-as-capacity.html
