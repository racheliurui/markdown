title: AWS - Simple Queue Service
date: 2018-04-16 08:57:23
tags:
- AWS
- SQS
---

# 058.mp4 -- SQS Overview

* up to 10 attributes can be add to a message
* Size 1 - 256 K
* Standard usage : provide cloudwatch metric (queue depth) to help  auto Scaling
* Queue type
   * Standard queue
   * FIFO (Max 300TPS, exact once) ; not available in all regions
* Message Lifecycle (Visibility Timeout)
* Dead Letter Queue : must be in same region under same account with the source queue
* Delay Queue
   * Define "DelaySeconds"
   * max inflight msg = 120000
* Message Timers : individual message being available with a delayed manner. set "DelaySeconds" for individual message

* Two type of polling
   * short polling : 有消息吗？服务器答： 有/没有
   * Long polling: 有消息吗， 20秒内有消息回我，我等着。 服务器：好的。
      * long polling减少无效的空返回 and false empty response (subset of servers)
      * set "WaitTimeSeconds" 1~20 second



How the "DelaySeconds" change affect existing Message (different behavior for Standard and FIFO)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-delay-queues.html

Delete the Queue : what happens to existing Message
https://docs.aws.amazon.com/cli/latest/reference/sqs/delete-queue.html

Message Retention & Visiblity timeout
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-limits.html

Short Pooling definition : (Subset of servers)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html

Queue identifier (format) https://sqs.regionname.amazonaws.com/accountnumber/queuename.uniqueforuser[.fifo]
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-general-identifiers.html

Message ID vs Receipt Handle
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-general-identifiers.html

How to check queue Depth (GetQueueAttributes & ApproximateNumberOfMessages)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_GetQueueAttributes.html

Dead letter queue limitations (origional queue type impact dead letter queue type; region & account limitation)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html

Cloudwatch integration with SQS (how often metrics are pushed; how to tell if a queue is active; no charge; support all queue types)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-monitoring-using-cloudwatch.html

Integration with CloudTrail (what will be loged)
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/logging-using-cloudtrail.html

Message Visiblity Range
https://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ChangeMessageVisibility.html
