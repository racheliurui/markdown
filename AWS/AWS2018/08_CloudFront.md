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
* CloudFront only cache GET and HEAD requests, for POST,PUT,DELETE cloudfront will only proxy
* Different combinations to cache for static / dynamic websites
  * For static, we can select only cache documents , exclude HTML/css/code; or set TTL and cache all
  * For dynamic, we can select only cache static content , exclude php/code ; or set TTL and cache all


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


# Using Amazon CloudFront For Your Websites & Apps

## References

> https://youtu.be/gUAuhdtHacI

* Tangible things to take away

## Best Practise to set up your origin

* Use Rout53 Health check and DNS failover for your origin
  * Enable latency based routing to multi edge locations
    * Sample scenario: 1 single region app act as origin for 4 cloud front edge in 4 regions
        * Improved: multi origin in multi regions, one origin down, the cloud front will fetch origin from avaiable origin
  * If you enable, Route53 will do health check automatically agianst your origin and if the backend is not healthy, the request is automatically detoured to other healthy origins
* Configure multi origins
  * Setup multiple origins --- dynamic contents to application and static to S3
* Secure your origin
  * If content is hosted via S3, by limit origin via OAI (Object Access Identity) only from could front, it will improve performance and protect the S3
  * If content is hosted via backend and with cloudfront cache, limit the origin at back end via white list cloudfront's IP address.
      * How to automatically update these addresses : subscribe AWS official SNS, when ip changes use lambda to parse the notification and update security groups.
  * Prevent backend overload
* Log request IDs
  * Generate request id and log to help debugging (???)
* Set origin response headers
  * Strict-Transport-Security:  max-age=15552000
     * tells browser to only send request via https, even address is http, the browser should change it to https.
     * helps with downgrade attacks
  * X-Frame-Options: SAMEORIGIN
     * used to tell if the response can be put inside an iframe. If yes, then the content can be hi-jacked and embeded into other website's iframe webpage.
  * X-XSS-Protection: 1; mode=block Options
     * means to enable the XSS protection
  * Cache-Control: max-age=300;public


Demo: create security group to allow all cloudfront ip address, add the security group to VPC subnet allow list where the real origin sits inside the subnet; use lambda to update the security group based on AWS published clouddfront list (SNS)

## Gaining visibility into your distribution

Scenario: monitor cache hit/miss directly using cloudfront's report view.
Scenario: look at cache statistics: "Percentage of GET Requests that Didn't Finish Downloading" is surging up.
   * Check the Popular Objects, ranking by "Incomplete Download", possible reason is that file is too large and not segmented.
Scenario: Check the "Viewers"  to check customer by location (country)
Scenario: Check the "Viewers" to check the customer by device

Scenario: how to identify bots? Create log to cloudwatch, filter log by bots and create alarms
Scenario: how to disable SSLv2 gracefully ? Create log to cloudwatch, filter log to check how many user are using SSL v3, then decide wether to turn SSL v3 on.

## how to improve cacheability

### Versionning website assets

* Two ways to version cached assets,
  * Use version number www.sample.com/v1/css/style.css; www.sample.com/v2/css/style.css
  * Use md5sum www.sample.com/css/style.css?<md5sum>   (How it works?)

* Benefits
  * monitor the error percentage when release new version, easy to roll back once something is wrong.

* Common CloudFront Cache Strategies
  * js, css, image, set to 1 year (as long as it can be)
  * index.html (no cache, max age = max session age) -- make sure unique session id is generated for each unique user
  * live streaming (max-age =2 , make sure user will follow the streaming )

* Shared assets from multi properties
   * Single S3 as origin for different domain, one .org and one .com

* Forwarded values
  When request hits cloudfront, still some dynamic content needs to visit the backend origin , then forwarded values will define with header will be forwarded, (like jsessionid)

* Invalidate the cache from cloudfront
  * it will only invalidate cache in cloudfront ,won't touch cache in client browser side

## How to test your configuration

* Test In Development Mode
   * Set TTL to 0 (then request will go through cloudfront but without caching)
   * Use WAF to white list only office IP
   * Chrome dev mode to check the unique headers added by cloudfront
* performance test
    * Backbone Testing , network provider to test from datacenter
    * Last Mile Testing , network provider to help test from end user
    * Real User Testing , use real web site user performance by injecting some scripts
* Load testing
    * traditional load testing is from one client which is hard to simulate DNS load balancing and real user env.
    * Better use distributed client with different ip in different region
* SSL Lab
    * Helps you verify your ssl config
    * https://www.ssllabs.com/
