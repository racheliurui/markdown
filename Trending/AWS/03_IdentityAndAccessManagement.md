title: AWS - iam
date: 2018-02-07 09:13:23
tags:
- AWS
- IAM
---


# IAM Overview

IAM: __Identity and Access Management__

## 010.mp4 overview



## 011.mp4

* Understand the difference between AWS IAM and customer IAM
* Understand the difference between aws account and aws iam users
* IAM is a service.
* IAM control access by policies which is organized by "statement", it include : resource (like a table, ); action(like access database); effect (like allow)
* security compliance: Payment Card Industry (PCI) Data Security Standard (DSS)
* Auditing : using CloudTrail

credential Report: downloaded excel (the report is generated every 4 hours, so there's delay)



### User


 如何表示user：
   * arn: amazon resource name， 格式，
       arn:aws:iam::account-ID-without-hyphens:user/Bill
   * id --如果用户是命令行创建
   * 普通的unique的user name
   * never give root access

credential的类型：
   * 密码
   * access key
   * SSH key
   * Server Certificates

用户re-name，
* console是不支持的，但是可以用CLI；powershell或者API


### Group

   * 不能nest group
   * 每个account最多100个group

### Roles

   * EC2 定义roles来访问其它aws 服务
   * 赋给一个account下的user去访问其它account下的资源

Role的定义包含：

   * RoleName: unique name within the current aws account
   * RoleARN: unique within whole AWS
       * arn:aws:iam::<uniqueaccountid>:role/<RoleName>
   * Trusted Entities: the service which ???


### Identity Federation

   * 正常aws一个account只允许5k user，但是做了id federation，就不受5k的限制了。
   * ways to do ID federation
      * amazon cognito
      * OAuth2
      * SAML2.0
      * LDAP 或者 AD

# 012.mp4

* Create User and groups
   Group can attach policy, and users will be added to Group(s)
   support bulk creating users ( and each user can have their  access key and secret access key which means password )
* Create account password policy
    * The password policy is configured at account level, and limitation is for users under that account
    * All users under a certain account will have a unique IAM url used to sign in.
* Create role
    Role can represent services under current account, to create a role mean give certain service under current account to access to other resources. (create a role represent certain type of resource, then apply policy to the role to giva access to resource, last???--which missed in the video, I guess should be assign role to instance of certain service. then that service will have access to resource which being defined via policy)
    Role can delecate the access to resource to allow access between different aws account
* download credentials report

## Identity-based policy

* Identity-based policy ： 可以attach到user，group，role上的policy
  * Managed policy：定义好以后可以复用的policy
    * aws managed
    * customer managed
  * in-line policy: hard code在user，group或者role的定义里的，不能重用的policy

aws managed policy
https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html


# 013.mp4 Trusted Advisor Service

Trusted Advisor Services

* Cost Optimization
  check unused EC2, RDS, Elastic LB, EBS, etc.
* performance
   check bottleneck Services
* security
   Check all security related config
* Fault torlerance
   Check all failover related config

can setup to send notification regularly to corresponding emails.

# 014.mp4 IAM Best Practise

* Enable MFA and reduce root access
  * support google athenticator
* Grant least privilege (start from nothing)
  * default deny
  * try not using wildcard *:* policy
  * try to use policy template
* Create individual Users
* Manage permissions with group
* Use IAM roles to manage permissions accross account and between Services under your account
  * external ID condition in policy to allow 3rd party
* Restrict further with conditions (for example, must use MFA when access certain api)
  * put condition to policy
* Use a strong password policy (expiration, length, forbidden re-use )
* Rotate the password credential
  * use credential report to identify
  * allow user to rotate credentials
* Enable AWS CloudTrail
* AWS Key management Service
* VPC security


## Exercise:

* ARN form.
* How to define policy for a website hosted on AWS
* maximum number of IAM access keys per user?
* one account can have only 1 alias
* Users need their own access keys(not password !!!) to make programmatic calls to AWS using the AWS Command Line Interface (AWS CLI), the AWS SDKs, or direct HTTP calls using the APIs for individual services.
* Policy sample definition (009.html)


## others

Sign in page url:
https://My_AWS_Account_ID.signin.aws.amazon.com/console/
