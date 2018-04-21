title: AWS - Route 53, CloudFront and S3 handson
date: 2018-04-18 10:04:59
tags:
- AWS
- Route 53
---

# 073.mp4 074.mp4 -- handson with S3 and Route53

* purchase a domain
* create a S3 bucket (use the domain name as bucket name)
* Download a HTML5 website
* update the website files to S3
* Edit S3 configuration : "Static Website Hosting" --> select the index.html
* Click the website , verify the origional host name endpoint works
* Enable versioning
* Delete index.html then use version control to revert the deletion (by deleting the deletion marker)
* upload a different version of index.html and then revert the update(by deleting the new version)
* Edite the life cycle rules: for revious version, after 30 days to Glacier and after another 30 days, delete permenently.
* create another S3 bucket (using the subdomain name with www. prefix)
* instead of hosting website, for this new S3 bucket, select re-direct all request to another host (the domain name we purchased)

# 075.mp4 -- Bring in CloudFront

* CloudFront can front web ; and  RTMP (Real Time Message Protocol), 流媒体
* Select CloudFront for Website
   * Source is S3 Bucket domain name with content files in it
   * Origin Path used to filter out which directory need to be cached
   * select allowed Protocol(Http/Https) and methods
   * TTL (set max,min,default)
   * Distribution Settings (how many edge location need to distributed to )
   * CNames: put puchased domain name
   * set Logging and comment
* wait till status to "Deployed" , check the cached contents
* use the CloudFront assigned domain name to visit the website
* trigger a "Invalidate" request to certain file or folder that matched with your request list.

# 076.mp4 -- Bring in Route53

## Demo1,
* Route53 --> Hosted zones
* select the puchased domain and click "Create Record Set"
* Because the website hosted inside aws, select "Use Alias"
* Choose the target as the CloudFront url
* hit the domain name from browser verify it works

## Demo2

* delete the existing record set
* create a new recordset without using Alias, pointing to a public IP which is the EC2 hosting a wordpress web application  

## Demo3 : Failover

* Change the routing policy from "simple" to "weighted"
* Give "DevID-Main" a weight 100 (0-255)
* Create another record set to pointing to CloudFront with id "DevID-FailOver" and weight 0

## Demo4: Health check

* delete other records only leave the one pointing to EC2 ip
* change the routing policy to "FailOver" and assign this target as "Primary"
* Assiciate the record set with Health Check definition
* Create another record poiting to CloudFront ; with routing policy to "Failover" and "Secondary"
