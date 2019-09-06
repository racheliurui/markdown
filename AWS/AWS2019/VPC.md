title: AWS - VPC
date: 2019-08-05 11:37:23
tags:
- AWS
- Transit VPC
- Transit Gateway
---


# Reference

> https://youtu.be/ar6sLmJ45xs

* North-South :
* West-East :

## Challenge with current VPC architecture

* lots of VPC and lots of connections and lots of peering
  * VPC peering : can't transit
  * Transit VPC (VPC with 10.1.0.0/16 and 10.2.0.0/16 go through transit VPC of 10.0.0.0/16)
  * Transit Gateway (2018)


## Transit Gateway (2018) -- tgw

   * Centralize VPN and AWS Direct Connect
   * 5k VPC across accounts
   * Flexible
      * Control segmentation and sharing with routing
   * Compared with transit VPC
      * AWS build in service

* AWS HyperPlane
  * Backbone of NLB, NAT Gateway, EFS and now Transit Gateway
  * Region wide scope

### Demo

![vpc_flat](https://github.com/racheliurui/markdown/blob/master/AWS/AWS2019/images/VPC_tgw_flat.PNG?raw=true)

* Flat : Every VPC should talk to each other.

![vpc_isolated](https://github.com/racheliurui/markdown/blob/master/AWS/AWS2019/images/VPC_tgw_isolated.PNG?raw=true)

* VPN: all traffic need go through VPN

### Reference Network Architecture


![vpc_arch](https://github.com/racheliurui/markdown/blob/master/AWS/AWS2019/images/VPC_tgw_reference_arch.PNG?raw=true)

### New Feature: VPC Sharing and Resource Access Manager

* external account managing public subnet
* internal account managing private subnet
* by sharing vpc across different account, make the resource more flexible and avoid VPC peering in some cases

### Segmentation considerations

* SG and IAM are effective and proven
* Shared VPCs vs VPC peering : shared VPC can across multi-account
* Separate VPC + Transit Gateway : simplest design without scaling issue (peering , VPC, routes)

### Sharing considerations

* VPC peering (max 100 VPCs); support inter-regions
* AWS PrivateLink : Supports overlapping CIDRs (using ELB)
* AWS Transit VPC : Shared seervices as a spoke
* Transit Gateway :  most advanced option

### Connecting to on-premises

* Virtual Private Gateway VPN
* Direct Connect
* Customer VPN
* Transit Gateway VPN

### 43min : an advanced use case (???)

### Reminder

* existing DMZs moving to cloud might not be a good idea
