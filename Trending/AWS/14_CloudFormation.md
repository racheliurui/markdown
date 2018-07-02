title: AWS - CloudFormation
date: 2018-04-12 16:38:23
tags:
- AWS
- CouldFormation
---


# CloudFormation Overview

JSON format definition about what service needs to be deployed
Infra as code


* Support JSON or YAML format
  * AWSTemplateFormatVersion : Format Version
  * Description
  * Parameters : define some params at stack creation time
  * Mappings: some predefined key/value pair, e.g, can be used to refer to value by region in resource def section
  * conditions , for example , if envcode=prod, then mount the disk when define the ec2 instance
  * "Resources" is the only mandatory section in CloudFormation def.
  * Outputs: can be used to be imported into other stack or report/show on console
* CloudFormer
  * only support JSON
  * export current account's selected service into cloudformation definition
  * Visual Tool to edit the cloudformation json template



  Sensitive Parameters (NoEcho)
  https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html


  Automatic Roll Back on Failure
  https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/troubleshooting.html

# 049.mp4 050.mp4 051.mp4 - hands on cloudformation

* Create cloudformation template using JSON editor
  * manually add "AWSTemplateFormatVersion","Description","Parameters","Resources","Outputs"
  * pre-defined copied: define a DynamoDB with
      * Parameters: ReadCapacityUnit and WriteCapacityUnit
      * Resources: DynamoDB Table with attributes definitions
      * Outputs: print out the table name
  * from AWS console , from "CloudFormation" service main page, select "create a new stack"
      * template can from sample / upload one to S3 / from a URL (hosted in S3)
      * Roll back on failure is "On" by default
  * swith to DynamoDB console, and check the table is created as expected.


* Create Cloudformation using CloudFormer
  * Create a stack and select "template from sample" and choose the "Cloudformer" template
      * give user and password to login CloudFormer (took 8 min to create from video)
      * output contains the url of the CloudFormer webpage
  * Login CloudFormer
      * create template
      * go through the wizard to select from existing resources as template to form the new template
      * note: it won't define paramteters (directly copy from the existing resources)
  * Launch the stack using the template created by CloudFormer

# CloudFormation Desinger

* Visualize the Design
* Can be used to update existing stack

# Extend with custom resources

Capability to create resource that implement aws defined create/update/rollback/delete and metadata

Standard Senario: use lambda

# Security

Policy example
* limit an user to only have access to create stack using template from certain bucket
* limit a user to only can update a stack using a specific yml from a certain bucket

IAM policy example
* limit type of resources the user can create

# Best Practise

* reuse accross Region
* Pseudo parameters (make use of the environment parameters)
* Use Mappings
* Use Conditionals


# References

>047.mp4 048.mp4

> CloudFormation Designer
>https://youtu.be/fVMlxJJNmyA
