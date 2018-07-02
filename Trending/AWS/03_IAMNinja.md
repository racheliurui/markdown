title: AWS - IAM
date: 2018-06-29 09:13:23
tags:
- AWS
- IAM
---


# Overview


The policy language,
* Specification: define access policies
* Enforcement: evaluating policies

## Policy Specification

### Speicification: PARC Model

* AWS Policy using PARC model,
 * P, Principal
   * can be aws user, group or role, service, or federated user
   * https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_principal.html  
 * A, Action
   * thousands of actions
   * Understand difference between __NotAction__ and __Deny__. (Important).
 * R, Resource
   * arn representing aws services
 * C, Condition
   * multiple conditions will by default using OR

### Policy Variables

* Policy version is mandatory, if not include, all variables will be treated as string

## Policy Enforcement

* Request raised, AWS will retrive all policies associated with user and resource
* Filter retrieved policy using action and conditions
* Evaluate all Deny policies firstly
* Evaluate all Allow policies, if find true statement then Allow, if not then Deny.

### Demo1, limit user access to his own home folder

### make use of "limited" IAM administrator

Demo1, A user which can create user but only attach certain list of policies
* Apply policy to access to IAM
   *  give user list user access ; give user full user access to self
* Apply policy to create user and attach policy ( use condition to limit the list of policies)

Demo2, Demo Grant Conditional Cross-Account Access
* Define a policy in PROD account represent what kind of access and attach the policy to a role
* Define another policy in PROD account to define which principal can consume the role
* Define a policy in DEV account to certain user to consume the role
* user can switch (like gmail switch user)

# Improvements

## EC2 fine grained policies

* resource represent ec2 resource based on resource arn till the instance id
* use tag in conditions

Demo3, limit user from starting/stopping/terminating instance unless he owns that instance

* EC2 will have owner tag
* Policy grants user access to EC2 console
* Policy limit user access based on owner tag

Demo4, Limit user from starting expensive instances

* Make use of "NotAction" and "NotResource" to make sure we don't miss out necessary access to launch a instance
* Define allow to certain action and certain resource and using condition to limit certain types

Improvement: make use of IfExists

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:StartInstances",
                "ec2:RunInstances"
            ],
            "Resource": "arn:aws:ec2:*:12234567890:instance/*",
            "Condition": {
                "StringNotLikeIfExists": {
                    "ec2:InstanceType": ["t1.*", "t2.*", "m3.*"]
                }
            }
        }
    ]
}
```

## Policy simulator

* Test your policy


## Decode authorization message (need access)

* use cli to decode
* Json Lint



# Reference
https://youtu.be/y7-fAT3z8Lo
