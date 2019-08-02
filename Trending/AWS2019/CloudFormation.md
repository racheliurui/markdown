title: AWS - CloudFormation 2019
date: 2019-08-01 11:37:23
tags:
- AWS
- CloudFormation
---


# what's New

* more resources including Alexa and custom resource


## Managing enterprise complexity

* Seamless handling secrets
* StackSet -- overide

## Improved handling of secrets

* Use SSM to handle dynamic parameter

```yaml
MasterUsername: ''{{resolve: secretsmanager:MyRDSSecrets:SecretString:username}}

```

* AWS Cloudformation Macros
  * Iteration
  * Transformation

* CloudFormation Linter
  *  Scripted --> Declarative --> DSLs --> Imperative
