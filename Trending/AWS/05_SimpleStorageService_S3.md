title: AWS - S3
date: 2018-04-09 09:57:23
tags:
- AWS
- S3
---


# S3 Overview


S3 is a webstore , not a file system !!

S3 -- writing concurrency
* "Eventually Consistency": make sure concurrent write will eventually get synced
* "Read after Write Consistency": make sure read access do not need to wait until write consistency is archived.

## S3 Event Notification

* Configure when event is triggerred
* Cifigure filter : File Prefix (Path and name) , Suffix
* Notification can integrate with Lambda, SQS , SNS

## LifeCycle Management

S3 --> S3 IA --> Glacier

* S3 IA has same durability 11 9's with S3 (Availability is lower as 2 9's)
* You can move object to IA by life cycle policy or direct upload with parameter to specify to save in IA

## Cross region replication

* ACL (Access Control List ) policies, if it's S3 encryption , it will be copied ; if it's using KMS then you have to handle manually by your self
* Any existing objects before turn on this feature needs to be copied manually to new region (only replicate new PUT)
* __Versioning must be enabled__
* The remote bucket can be managed by different user
* Can replicate objects only with certain prefix (folder)
* If delete object and specify the version , then the remote bucket won't delete that object (have to manually );
    * From another material, it's said "delete and life cycle actions are not replicated"

* You can specify A bucket replicate to B and then specify B replicate to A. This will result in A and B synced.
* Once the replication is turned on , you can head-object to S3 object and check the ReplicationStatus metadata
* Very useful feature when you want to accelerate upload to a remote region, upload to local region then replicate to remote

![cross Region Replication cli](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/05_S3_CrossRegionReplication.png?raw=true)

![cross Region Replication policy](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/05_S3_CrossRegionRepli_Policy.png?raw=true)



# 026.mp4 - hands on demo on S3 Polices and ACL

* Add/Edit/Delete bucket/bucket object permissions (which grantee has what access) --either at bucket level or object level
* Create bucket policy (JSON) -- AWS Policy Editor or copy from samples -- only available at bucket level


How to spcify a S3 object using ARN, for example
arn:aws:s3:::examplebucket/developers/design_info.doc

> https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html

AWS S3 RRS option
https://aws.amazon.com/s3/reduced-redundancy/




S3 Bucket URL
https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html




Difference of Durability & Availability & Concurrent facility fault torlerance
https://aws.amazon.com/s3/reduced-redundancy/


How to share object with others via pre-signed URL
https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html
* use (your credential + expiretime) to create the url


Difference between : (ACL is legacy method to control access to S3)
Bucket ACL/policy
Object ACL/Policy

Error Message code,
https://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html
For durability, RRS objects have an average annual expected loss of 0.01% of objects. If an RRS object is lost, when requests are made to that object, Amazon S3 returns a 405 (Method Not Allowed) error.


## Best practise to max the S3 performance

### Key naming
Useful when your S3 reach 100 TPS (100 request per second)
https://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html
* S3的object的path类似于key，为了能够均匀分布使得存取性能优越可以：
1） 加hash在key前面
2） reverse key string

Sample of anti-pattern

All the files will be saved on same partition which not favor of concurrent searching. Especially when query reach 100TPS , must not allow below naming converntion.

```
<my-buckt>/2018-0601-001.png
<my-buckt>/2018-0601-002.png
<my-buckt>/2018-0601-003.png
<my-buckt>/2018-0602-001.png
<my-buckt>/2018-0601-002.png
```


### __S3 Transfer Accerleration__
* Once enabled, it will give a new endpoint;
* it support automatically route upload to closest location.(Make use of Edge Locations)




###  multi part uploading
https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html

Get request: put Range in the header

### Make use of CloudFront  

Difference between ElasticCache and Cloudfront --- which is the destination to cache S3 objects?
ElastiCache uses redis and memcached to improve the performance of web applications by allowing you to retrieve information from fast, managed, in-memory data stores, instead of relying entirely on slower disk-based databases.

While CloudFront is a global content delivery network (CDN) service that accelerates delivery of your websites, APIs, video content or other web assets

https://acloud.guru/forums/aws-certified-solutions-architect-associate/discussion/-KbKW7V5i1NIZ6n9S_TK/what_is_the_difference_between


S3 detect data corruption
Content-MD5 Checksum and CRCs to detect data corruption
https://aws.amazon.com/s3/faqs/#

# Hands-On

* Enable , disable versioning at bucket level
* Before enable version , object has default version id of __null__
* Delete & revert deletion of object
* List versions for an object, apply a selected version
* Add life cycle rule to selected bucket: which scope in the selected bucket will be moved to what storage after defined days and be deleted from S3 after another defined days.
* Enable logging ( S3 access related logging )
* Add tag to S3 bucket (easy management)
* Define cross region replication
* Define events -- SNS/SQS or Lambda
* Requester Pays

## New feature

Retrieve data within minutes from Glacier
https://aws.amazon.com/about-aws/whats-new/2016/11/access-your-amazon-glacier-data-in-minutes-with-new-retrieval-options/

S3 Analytics :  storage data analysis
  * Category the access pattern by object prefix,name,bucket or tag to help you create proper rule

S3 Inventory:
  * Save the money of List API when you have loads of object.Generate List every day (or week depending on your config)
  * need to config policy to allow S3 to write inventory list report into your bucket

Object Tag:
  * Max 10 tag per object
  * Tag can be used in LifeCycle management, Access Control or Storage Analysis

CloudTrail: S3 Data Events:
  * Object level events
  * Security and Audit purpose

CloudWatch: S3 Metrics
  * Not free; $0.3 per metrics
  * every 1 min

## VPC endpoint for S3

* Security Control
  * VPC can specify allow certain action to specific S3
  * S3 can specify allow  which vpc to access it.

# Reference

> 024.mp4 - S3 overview
