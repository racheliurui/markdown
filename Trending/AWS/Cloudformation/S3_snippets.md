title: AWS - Cloudformation for S3
date: 2018-07-08 11:13:23
tags:
- AWS
- CloudFormation
- S3
---


# Template for S3 bitbucket

* Delete the stack will delete the S3 bucket

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: "Sample CloudFormation Template: this template will define a statck with S3 bucket"
Resources:
   MyS3Bucket:
       Type: AWS::S3::Bucket
       Properties:
           AccessControl: PublicRead
           Tags:
             -
               Key: "S3BucketName"
               Value: "Dev"             
Outputs:
  BucketName:
    Value: !Ref 'MyS3Bucket'
    Description: Name of the sample Amazon S3 bucket with a notification configuration.
```
