title: AWS - Cloudwatch
date: 2018-04-17 15:41:23
tags:
- AWS
- Cloudwatch
---

# overview

__Cloudwatch Metrics is a time series data store.__

Cloudwatch Alarms : based on Metrics ; Metrics Threshhold combined with Evaluation Period will decide wether to trigger alarm
  * integrated with SNS, Email etc.
  * state: __OK;ALARM;INSUFFICIENT_DATA__



## Integrate Cloudwatch with 3rd party monitoring platform

Consider below carefully when integrate

* IAM Permission
* API :  There is a limit of how many metrics being returned by one request. The number equals to if you have metrics with 1min period, then each time you can only retrive 1 day's metrics. (24*60=1440 metrics).

```shell
aws cloudwatch list-metrics --metric-name EstimatedCharges
# period is in second, so it's 5 min
aws cloudwatch get-metric-statistics --namespace AWS/EC2 --metric-name CPUUtilization \
      --dimentions Name=InstanceId,Value=i-30c9605  \
      --start-time "2014-10-11T00:00:00Z" --end-time "2014-10-12T00:00:00Z" \
      --period 300 \
      --statistic  {"average","maximum"} | more
```

* Request Through put: change the retrieving amount by adjusting start and stop and period make sure the response fit in requirement. (???)
* Late Arriving data ( __BackFill__ feature)



## Cloudwatch logs

Centralized log (like ELK) for all AWS resources & services.

Has throuput limitation of 1MBps.

* It's built on __Amazon Kenesis__
  * Agent needs to be installed on server
    * install using wget (a python script)
    * Run the agent by providing : what log folder needs to monitor;log group name (for similiar log from a cluster); stream name (like EC2 instance id); timestamp format ; reading for start or end of the log file when initiated.
  * Monitor
    * Once agent started, we can monitor from AWS console.
    * Set data expiration for log group (storage cost money)
    * Set Metric Filter against the log item to create metrics
      * Filter Pattern : for example only filter out logs related to invalid user and create a metric based on that
      * support literal form; common log format ; json log (like CloudTrail)
    * For existing metric, you can create an alarm (invalid user login 2 times per 5 min) and send to email address
  * Access, arn for logs is : __arn:aws:logs:*:*:*__
    * No need to login the remote server but can tail the log using aws api (because it's already centralized)
  ```
  aws log pull --log-group-name /var/log/secure --log-stream-name i-30c960d1 | grep "invalid user"
  ```

* Cloudwatch Log source: EC2, CloudTrail, or other resources (like S3)
* Use cases:
  * monitoring for errors;
  * long term off box storage of logs;
  * tailing log without connecting to host;
  * correlate system status with change events. (correlate change with errors )


### sample use case to monitor S3 logs

1) Turn on S3 log , then the log will be sent to another S3 bucket
2) From EC2 run python script to pull and send the log to CloudWatch
3) From CloudWatch , configure log metrics

### sample use case to monitor cloudtrail

Fully integrated, no parsing needed, just enable cloudtrail , specify the target S3 bucket and enable cloudwatch integration.


### sample use case to integrate with EC2 configuration service

Windows servers can also be


## Cloudwatch Events (???)

Cloudwatch Event vs Cloudwatch Log , what's the relationship

    * source: By status change or by CloudTrail event
    * Rules:  matching
    * target: SNS/SQS; Kinesis; Lambda(limited region to support Lambda)

# Bill

$0.3 Per metric per month

# References

> 066.mp4 067.mp4
> 068.mp4 - hands on with cloudwatch

> https://youtu.be/pTzv-i1uvvE
