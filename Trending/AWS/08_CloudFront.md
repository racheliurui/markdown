title: AWS - CloudFront
date: 2018-04-10 15:13:23
tags:
- AWS
- AWS CloudFront
---

# 033.mp4 034.mp4 -- cloudfront overview

CloudFront = Content Delivery Network by AWS

CloudFront deployed on Edge locations (number of Edge locations>Available zones> Regions)

* Source can be S3 , HTTP Server on AWS or outside AWS
* CloudFront only cach GET and HEAD requests, for POST,PUT,DELETE cloudfront will only proxy
* Different combinations to cach for static / dynamic websites
  * For static, we can select only cache documents , exclude HTML/css/code; or set TTL and cache all
  * For dybamic, we can select only cache static content , exclude php/code ; or set TTL and cache all
