title: AWS - CloudTrail
date: 2018-04-22 11:55:00
tags:
- AWS
- CloudTrail
---

# Reference

https://youtu.be/vtMCjyE5nms

## AWS CloudTrail OFF IR (Incident Response) Runbook

When someone want to turn off the CloudTrail, it will automatically being turned on and automated report and reminder being generated.

Automation Steps,

1. Turn CloudTrail backon
   * using python/lambda to handle the turn off event and turn it back on.
2. Gather data related to "TURN OFF" incident
3. Extract principal, date, time, source IP from event data
4. Map principal who assumed the role
5. Lookup human contact info
6. Contact human provide guidance
7. Generate event summary for report

## questionair before implementation

1. What's my expressed security objective in words
2. Is it configuration or behavior related ?
3. What data, where could help inform me ?
4. Do I have requisite ownership or visibility ?
5. What are my performance requirements ?
6. What mechanisms support the above ?
7. What is my expressed security objective in code?
8. Am I done?
9. Does a human need to look at this? When?

## Demo - S3:PutBucketPolicy IR Runbook

When someone changed the S3 policy in a bad way, check the policy and restore it if needed.This runbook is making use of Stepfunction to implement it.

## Demo - EC2 Login IR Runbook (User Login)

When someone logged into the instance (as long as they have the key), check the login behaviour combining with other relavant data, then decide wether to isolate the instance or not.
This runbook is making use of cloudwatch event (cloudwatch will trigger a selfdefined json event when ec2 login action happens).

Identify

1. Get the user
2. Gather relevant data
3. Terminate session
4. Isolate the instance
5. Report the incident

Research

1. Pull logs
2. Correlate other data
3. Report findings

Forensics

1. Memeory Dump
2. Create AMI and copy to forensics account
3. Launch instance & Investigate & Report findings


## Demo


# CloudFront Deepdive -- SEC318

## Reference

> https://youtu.be/t0e-mz_I2OU

## overview

* A Real Sample with who when what to whom and where,
  * A Json format log: a user arn at xxx time made an api call "start log" to arn resource at sydney region and from xxx ip address with what kind of browser.
* Turn on Cloudtrail
  * You can specify a new s3 or exising one.
  * You can turn it on from web console or aws cli
  * Two steps to turn it on: define the trail and start the trail
* Aggregate multiple accounts' cloudtrial log into one bitbucket
  * <bucket_name>/optional_prefix/AWSLogs/AccountId/CloudTrail/Region/YYYY/MM/DD/filename.json.gz

## monitoring and notification

* Help prevent __high blast radius__ Behaviors
* You can make use of pre-defined transformation
   * Define cloudtrail to log api call into S3,
   * Define cloudwatch to receive the logs
* view the logs
   * Cloudtrail console can be used directly
   * Use aws CLI


## encrpt cloudtrail log

* The cloudtrail log by default is encrypted at serverside (using SSE-S3)
* You can chose to use KMS, create key , use key and assign decrypt access to log readers
   * Create key, get the arn for the key;
   * Attach policy to the key
   * Attach policy to IAM user/group to be able to use the key to do decryption
   * Update the trail to use the key
* You can use same key for KMS key for multiple accounts
   * When Attach the policy to the key , specify the key have access to encrypt trails belongs to multiple accounts ; then update trails for those accounts to use the key


## Validating the file integrity of cloudtrail logs

* Turn on by --enable-log-file-validation to trail
   * hourly generte digest file signed by CloudTrail
   * digest file diliver to s3 certain folder <bucket_name>/optional_prefix/AWSLogs/AccountId/CloudTrail-Digest
   * use aws cli command line combine the digest file to validate the log file or use your own tool
     * you need to have access to decrypt & view the log and digest key to do so



# 098.mp4 099.mp4 -- CloudTrail

Logs:
* who (user)
* when (Timestamp)
* action (api call)
* to whom (resource)
* where (region)



Helps : security , compliance , troubleshooting

* Persist into S3 as JSON format and using server side encryption.
* integrate with CloudWatch logs (to generate alarms or to send to Kinesis) and SNS



For CloudTrail logging for S3 objects,
  * $0.1 per 100000 data events
