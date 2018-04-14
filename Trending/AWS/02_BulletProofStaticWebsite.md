title: AWS - Handson Static Website
date: 2018-02-05 09:13:23
tags:
- AWS
---

# set up a bulletproof website with AWS


* demo-005 part2: use Route53 service to buy a domain
* demo-006 part3:
  * create S3 bucket, upload the files; host static website (and visit using raw s3 url);
  * create another bucket with www naming and redirect the request to naked domain (domain.com).

Tips, set the correct MIME type for uploaded files
https://developer.mozzila.org/en-US
search for "complete list of MIME type"

* demo-007 part4: use "Certificate Manager" service to create Certificate (domain.com and *.domain.com)
* demo-008 part5: create distribution using cloudfront service to help with security(D-DOS??),performance,fail over,attach certs
    * default TTL : default is 24 hours, means refresh from S3 every 24 hours
    * alternative domain names: the domain name purchased
    * SSL certificate: custom SSL certificate (created in previous demo)
    * default root object: like index.html
    * how to manually trigger a refresh: create a invalidation, and using "*" to specify invalidate everything.
* demo-009 part6: go back to "Route53" and configure the "Hosted Zones"
    * Create a "A-IPV4" typed "record set", set the url to domain.com, and alias target to the cloudfront endpoint.
    * Create another "CName" typed "record set", url to www.domain.com, and non-alias pointing to naked domain which will be routed to cloudfront endpoint.

## background www and naked domain names

https://www.sitepoint.com/domain-www-or-no-www/
