title: AWS - Cloudwatch
date: 2018-04-17 15:41:23
tags:
- AWS
- Cloudwatch
---

# 066.mp4 067.mp4 -- Cloudwatch overview

* Cloudwatch metrics
 * Not just resources/services but including billing
* Cloudwatch statistics (on metrics, with max,min,average)
* Alarms
  * integrated with SNS
  * Alarm Threshhold combined with Evaluation Period will decide wether to trigger an alarm
  * state: __OK;ALARM;INSUFFICIENT_DATA__
* Cloudwatch logs
  * Log source: EC2, CloudTrail, or other resources
  * log streams/groups/filters/Retentions
* Cloudwatch Events
  * source: By status change or by CloudTrail event
  * Rules:  matching
  * target: SNS/SQS; Kinesis; Lambda(limited region to support Lambda)

# 068.mp4 - hands on with cloudwatch

* EC2 console : check the cloudwatch console from monitoring tab. (basic mornitoring: 5 min refresh; Detailed monitoring: 1min refresh)
* click the metrics (and by switching the region , verified that the metrics list is different in different region)
* For each of the metric we can create Alarm
* go to dashboard, create a new dashboard --> create a "Metrics Graph" --> select metrics --> generate a new wiget on the dashboard
* try the cloudwatch events (new feature, with limited event source)
