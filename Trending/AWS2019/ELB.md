title: AWS - ELB
date: 2019-08-13 11:37:23
tags:
- AWS
- ELB
---

# Reference

> https://youtu.be/VIgAT7vjol8

## Elastic Load Balancing: Deep Dive and Best Practices - 2018

* Layer 4 and Layer 7 Load balancing difference,
  * Layer 4 support TCP; Layer 7 only support http and https(will terminate the TLS)
  * Layer 7 Connection will be terminated and pooled
  * Layer 7 Headers can be modified
  * X-Forwarded-For http header will be modified

* Product mapping
  * Application LB is layer 7 LB; Network LB is layer 4 LB


## ALB

  * ALB support Path and host based routing (single ELB dispatch all traffic) ; deep integration with EKS -- Micro Service Archi
  * ALB can do Redirects ; Fix response ; Slow start (configurable like 10 min) ; ALB IPV4 and V6 support;
  * ALB update certs
      * IAM to control who have access to update
      * Use ACM (AWS Certificate Manager) to directly push and rotate certs with ALB
  * Integrate with AWS WAF
  * Server Name Indication (SNI) : load balancing multiple applications that have muti certs
  * Authentication at ALB layer (OIDC, Cognito, SAML)

## Netflix Demo (27:40)
