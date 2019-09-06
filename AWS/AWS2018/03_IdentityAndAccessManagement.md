title: AWS - IAM
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
* IAM control access by policies which is organized by "statement", it include : resource (like a table); action(like access database); effect (like allow)
* security compliance: Payment Card Industry (PCI) Data Security Standard (DSS)
* Auditing : using CloudTrail
* Credential Report: downloaded excel (the report is generated every 4 hours, so there's delay)



### User


 如何表示user：
   * arn: amazon resource name， 格式，
       arn:aws:iam::[accountIDNum]:user/Bill
       * loads of examples:

> https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html

   * id --如果用户是命令行创建,则可以拿到用户id
   * 普通的unique的user name
   * never give root access

Credential的类型：
   * 密码
   * access key
   * SSH key
   * Server Certificates

用户re-name，
* console是不支持的，但是可以用CLI；powershell或者API


### Group

   * __不能nest group__
   * 每个account最多100个group

### Roles

   * EC2 定义roles来访问其它aws 服务
   * 赋给一个account下的user去访问其它account下的资源

Role的定义包含：

   * RoleName: unique name within the current aws account
   * RoleARN: unique within whole AWS
       * arn:aws:iam::<uniqueaccountid>:role/<RoleName>
   * Delegation:
       * Create a role in account that owns the resource; then attach policy to represent the role access; setup trust relationship (allow consume the role)

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
   support bulk creating users ( and each user can have their  access key(like public key in keypair) and secret access key(like secret key in key pair) which means password )
* Create account password policy
    * The password policy is configured at account level, and limitation is for users under that account
    * All users under a certain account will have a unique IAM url used to sign in.
* Create role
    Role can represent services under current account, to create a role mean give certain service under current account to access to other resources. (create a role represent certain type of resource, then apply policy to the role to giva access to resource, create trust relationship to allow other resource or user to consume this role.
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
* maximum number of IAM access keys per user? 2
* one account can have only 1 alias
* Users need their own access keys(not password !!!) to make programmatic calls to AWS using the AWS Command Line Interface (AWS CLI), the AWS SDKs, or direct HTTP calls using the APIs for individual services.
* Policy sample definition (009.html)


## others

Sign in page url:
https://My_AWS_Account_ID.signin.aws.amazon.com/console/

## Integrate with AD - Best Practise


![image of integrate with AD](https://github.com/racheliurui/markdown/blob/master/AWS/AWS2018/images/03_IntegrateWithMicrosoftAD.png?raw=true)

> https://youtu.be/Iu-CpNFMELs

3 Options:
* Option1, EC2 join in on-promise AD domain
   * Expose a lot of ports as needed by EC2 on cloud
   * AD connector
     * Initial Solution:  LDAP forward to on-promise AD
     * EC2 domain join
* Option2, Run AD on EC2 (Paas)
   * Trust model Or Replication Model
   * Support NetBIOS name resolution
* Option3, Use AWS AD (SAAS) (released at Dec 2015)
   * Trust model : no trust no replication
   * Work well with other Microsoft SAAS on AWS (MS SQL server)
   * one way trust and 2 way Trust
     * one way trust is used to access on cloud resource using local AD account
     * 2 way trust is used when cloud resource needs to access on-promise resource (say, printer)
   * limitation (at 2016): don't support ldaps; max 5w users

# Federation

## Options

### Option 1, Simple AD

* Microsoft AD Compatible;
  * Can't set up trust bettwen M AD and Simple AD
  * Don't support schema extention, MFA, ldaps
* Cheapest Option, suitable for <5000 users

### Option 2, Microsoft AD

* Support trust with M AD
* Don't support schema extention, MFA
* Suitable for >5000 users and need trust with on-premise M AD

### Option 3, AD Connector

* Proxy service to directly use on-premise M AD
* Support MFA

### Option 4: SAML : Security Assertion Markup Language  

* Identity provider and AWS build up relationship in advance
* login flow will use Identify provider's service to do the authentication

## How to plan the federation

Choose your SAML provider : for example ADFS, Active Directly

### Federation High Level Steps

![SAML process](https://github.com/racheliurui/markdown/blob/master/AWS/AWS2018/images/03_IAM_SAML.png?raw=true)

* Prepare SAML provider in your network
* Config SAML provider in AWS IAM
* Config Roles for your federated users
* Create Groups in AD matching IAM Roles
* Config SAML IdP (Identity provider) & create assertions for SAML auth response
* Post SAML assertion result to AWS login url

So when user raise a request, it will be mapped to a group, and that group is mapped to IAM role.
When a user belongs to multiple group, and then the group will be mapped to multiple IAM role, user will have option to signin with one role.

You can use aws CLI with SAML ( the session will be persisted by default for 60 min)

# AWS Active Directory deep dive

* When create, 2 options
 * Option 1, create AWS Simple Directory
     * When create , specify VPC and 2 subnets (in different AZ)
     * for linux to join in the AWS simple directory, need install SSSD and use SSSD to join in the domain (need reboot)
     * for windows, when create ec2 instance, you can specify the AWS simple directory
 * Option 2, create AD connector to on premise AD
     * pointing to MS AD (need VPN), and also need specify VPC and 2 subnets.


# References

> AWS AD federation
> https://www.youtube.com/watch?v=ytSjsEER-y0

> Best Practise of integrate with MS AD (2016 Nov)
> https://youtu.be/Iu-CpNFMELs

> AWS AD deep dive
> https://youtu.be/CY-xvo8Cc54




# Progressive Journey Through AWS IAM Federations Options - 2015 (SEC07)

> https://youtu.be/-XARG9W2bGc

## SAML Primer -- Security Assertion Markup Language

* Configuration Time:
  * Identity Provider and Service Provider to exchange metadata in advance
* Run time:
  * Cryptographic Trusted Assertion (login flow)

## Demo -- Automating onboarding

* Use python script to create providers, roles and policies into AWS

  * python script to create IAM provider
  * python script to create role with inline policy
  * python to generate ldif (ldap exchange file) file and load group definition back into ldap

* Use python script to create group definitions into Directory

## AWS Business Partner's Solution Demo

# Nova

* Solution quite like Ranger
* User authenticate request with Nova is routed to AD
* AD pass then authentication and reply user id to Nova.
* Nova query user group information from AD, map to AWS group saved in its database
* Nova reply customer ask for which role the user want to log in
* Nova request sts from IAM and return to user (either login aws console or get temp access keys)

# Nova 2

* add another dimention by using tag -- to label team / resource

# Nova 3

* allow user to select which account, which application and which environment to work on

# Basics

Identity based Policy vs Resource based policy:
Identity based：for a given user, define what resource he/she can access
Resource based：for a given resource, see who have access to it
