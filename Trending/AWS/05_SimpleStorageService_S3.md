title: AWS - S3
date: 2018-04-09 09:57:23
tags:
- AWS
- S3
---


# 024.mp4 - S3 overview


S3 is a webstore , not a file system !!

## cross region replication

* New feature
* ACL (Access Control List ) policies, if it's S3 encryption , it will be copied ; if it's using KMS then you have to handle manually by your self
* Any existing objects before turn on this feature needs to be copied manually to new region


# 025.mp4 - hands on demo on S3 Config

* enable , disable versioning at bucket level
* before enable version , object has default version id of __null__
* delete & revert deletion of object
* list versions for an object, apply a selected version
* add life cycle rule to selected bucket: which scope in the selected bucket will be moved to what storage after defined days and be deleted from S3 after another defined days.
* enable logging ( S3 access related logging )
* add tag to S3 bucket (easy management)
* Define cross region replication
* define events -- SNS/SQS or Lambda
* Requester Pays

# 026.mp4 - hands on demo on S3 Polices and ACL

* Add/Edit/Delete bucket/bucket object permissions (which grantee has what access) --either at bucket level or object level
* create bucket policy (JSON) -- AWS Policy Editor or copy from samples -- only available at bucket level


How to spcify a S3 object using ARN,
https://docs.aws.amazon.com/AmazonS3/latest/dev/s3-arn-format.html

AWS S3 RRS option
https://aws.amazon.com/s3/reduced-redundancy/

overage fees
https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html
Pricing for Amazon S3 is designed so that you don't have to plan for the storage requirements of your application. Most storage providers force you to purchase a predetermined amount of storage and network transfer capacity: If you exceed that capacity, your service is shut off or you are charged high overage fees. If you do not exceed that capacity, you pay as though you used it all.

Amazon S3 charges you only for what you actually use, with no hidden fees and no overage charges. This gives developers a variable-cost service that can grow with their business while enjoying the cost advantages of Amazon's infrastructure.


S3 Bucket URL
https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html

Bucket naming convention
https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html

S3 Object consists of,
https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingObjects.html

S3 Object naming
https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html

Difference of Durability & Availability & Concurrent facility fault torlerance
https://aws.amazon.com/s3/reduced-redundancy/

Format of the CORS configuration
https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html#how-do-i-enable-cors

How to share object with others via pre-signed URL
https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html
* use (your credential + expiretime) to create the url


About multi part uploading
https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html

Difference between : (??)
Bucket ACL/policy
Object ACL/Policy

Error Message code,
https://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html
For durability, RRS objects have an average annual expected loss of 0.01% of objects. If an RRS object is lost, when requests are made to that object, Amazon S3 returns a 405 error.

S3 event types
https://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html#notification-how-to-event-types-and-destinations

Best practise to max the S3 performance (Key naming)
https://docs.aws.amazon.com/AmazonS3/latest/dev/request-rate-perf-considerations.html
* S3的object的path类似于key，为了能够均匀分布使得存取性能优越可以：
1） 加hash在key前面
2） reverse key string

Difference between ElasticCache and Cloudfront --- which is the destination to cache S3 objects?
ElastiCache uses redis and memcached to improve the performance of web applications by allowing you to retrieve information from fast, managed, in-memory data stores, instead of relying entirely on slower disk-based databases.

While CloudFront is a global content delivery network (CDN) service that accelerates delivery of your websites, APIs, video content or other web assets

https://acloud.guru/forums/aws-certified-solutions-architect-associate/discussion/-KbKW7V5i1NIZ6n9S_TK/what_is_the_difference_between


S3 detect data corruption
Content-MD5 Checksum and CRCs to detect data corruption
https://aws.amazon.com/s3/faqs/#
