title: AWS - CloudFront
date: 2018-04-10 15:13:23
tags:
- AWS
- AWS CloudFront
---

# Terminology

Canaries: when deploy limite the new version into a contained env until fully tested then roll out.

CloudFront = Content Delivery Network by AWS

__CloudFrontPop__: Point Of Presence, Edge Location
  * Located in DataCenter (Major metropolitan) with direct connection with multi ISPs
  * Terminating Viewer connections
  * Request routing to CloudFront is mainly done by __DNS Layer__

CloudFront deployed on Edge locations (number of Edge locations>Available zones> Regions)

* Source can be S3 , HTTP Server on AWS or outside AWS
* CloudFront only cach GET and HEAD requests, for POST,PUT,DELETE cloudfront will only proxy
* Different combinations to cach for static / dynamic websites
  * For static, we can select only cache documents , exclude HTML/css/code; or set TTL and cache all
  * For dybamic, we can select only cache static content , exclude php/code ; or set TTL and cache all


# How it works

Cache Key : generated using request URL (remove query string, protocol  and add encoding)

Specify ExpireTime: sent from original header
Specify Max Age: for example max-age=300 means 5 min

## CloudFront Configurations

* Config __Cache Behaviors__

# Best Practise

## Try to use ECS-Enabled Resolver

![CloudFront Common Issue](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/08_CloudFront_Issue.png?raw=true)

Issue , when View send request to ISP, ISP will forward the request to AWS and lost the original viewer's location information, and aws Route53 will only return the Edge Server which has smallest latency with the ISP instead of the viewer.

Resolution:
* Use local resolver
* Use Resolver that Support ECS (EDNS0 Client Subnet)--->DNS Query include requester's network info
* Some resolver like google support this new standard.

## cache the error page

When source returns error page, that static error page can also be cached.

## Cache Dynamic content

Set TTL =0 (???)

## Version your contents

If you don't, then the new version might not be relfected. Version your contents means different version of content have different url

## minimize ForwardHeaders values

Forwarded headers will be used as part of Cache Key

## Debug --> Use the CloudFront Logs

Log your request ID

## Use pre-configured WAF Rules with WAF to protect CloudFront

## Protect Private Content


How to stop user directly access your origin
Option 1: Prevent direct connect to origin (OAI, Origin Access Identify)
Option 2: Origin only accept request from Caching layer (limit source IP address)


Use signed URL to restrict access to certain file (no correct url no access)
Use signed cookie ; restrict access to multiple file (module)

# Monitoring

Metrics: Server-side Metrics; Canaries;  3rd party http tests

# Design Pattern for Availability (???)

__Food Tasting__
__Flash Crowds__
__Defence In Depth__
__Time Bomb Jitter Protection__

# RUM (Real User Monitoring)

## Synthetic Monitoring
* Synthetic Monitoring
  * Simulate client to test the performance of the application ; can use as baseline of the application
* Consistent Signal of Service Health

## RUM
* Script Injected in webpage to send data back and aggregate the data.
* Analysis RUM
   * Use CloudFront
   * Use Multi Region
   * Use HTTP/2 (support multi thread to serve multi objects at the same time)


# Reference

> 033.mp4 034.mp4 -- cloudfront overview

Design pattern for HA, lessons from AWS CloudFront (???)
> https://youtu.be/n8qQGLJeUYA

CloudFront Best practise
> https://youtu.be/fgbJJ412qRE
