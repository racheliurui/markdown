title: AWS - CloudTrail
date: 2018-04-22 11:55:00
tags:
- AWS
- CloudTrail
---


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
