title: AWS - Amazon Route 53
date: 2019-07-23 11:37:23
tags:
- AWS
- Route53
- Hybrid Cloud
---

# Route53 Resolver (Released in 2018.12)

*  Issue: in hybrid architecture, VPC can't access Data Center name and Data center can't access VPC private DNS name.
*  Traditional workaround:
    * spin up EC2 to run bind or unbound as DNS server, used to forward request to plus-2 resolver
    * need to consider failover and sometimes a group of DNS server per vpc
*  This requirement is called Recursive DNS lookup.

## How Route53 Resolver works

* only works for single region (can't span region)
* multiple VPCs under multiple accounts (as long as they are in same region) can share the same Resolver endpoint
* Need to provision ENI for the resolver, for HA and performance, recommend to provision multiple ENIs
   * One ENI serving one direction of querying (for example, from VPC to On-Pre)
* When a resolve request received, it will check against all resolve rules, if no matching, treat as local.
   * rules can be shared between accounts (via Resource Access Manager  -- RAM)

## Route 53 Resolver Demo

* Resolving sequence
    * Auto defined Rules: VPC / Private Hosted Zones/ Internet Resolver
    * Extra rules
       * tips, have "." rule work as default query forward rule, anything not fit in auto defined rules will go to ns.mycompany.com
       * tips, ns.mycomany.com have a "." rule to recursive request to internet if no rules matched
       * tips, a rule to __forward__ any request to acquriedcompany.com to ns.acquriedcompany.com

* API used to create endpoints;
   * Endpoint need to have attached security group to allow port 53
   * API to create rule
   * API to share defined rules

* Monitoring: Cloudwatch and CloudTrail



# Terminology

Authoritative DNS
Recursive DNS



# Reference
> https://youtu.be/D1n5kDTWidQ
