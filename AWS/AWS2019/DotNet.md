title: AWS - DotNet
date: 2019-08-15 11:37:23
tags:
- AWS
- DotNet
---

# Reference

>
https://youtu.be/FteCJQcTDc4

## Modern .NET applications on AWS


Mosaic image
* Service being used: .net tool, lambda, xray, ecr fargate, dynamodb, cognito, s3, code pipeline, sqs, stepfunction, aws batch, ssm param, cloudformation

### Demo : use visual studio to CICD


* AWS Batch
  * Work as queue; ability to use EC2 Spot Instances
* Use Visual Studio, you can directly publish the code to generate Docker image and publish to AWS ECR
   * The code logic is to download the pic, and upload to corresponding S3 Raw folder
* Use Visual Studio, directly publish Lambda function
   *  In lambda , register XRay will enable XRay drill down details of the invoke
* Code Pipeline
* Use step function to link all the functions
