title: AWS - Glacier
date: 2018-06-17 12:31:00
tags:
- AWS
- AWS Glacier
---

# Terminology

__Vault__ : Container for Archives. 1k Vualts per account
__Archives__: Basic Unit of backup. 40TB Max per archieve. No limit on number of archives.
__Inventory__: Cold __index__ of archives (refresh every 24 hours)

# Access Glacier

1) SDK / API
2) S3 Lifecycle (Bucket Level or Object Level)
  * new feature: Archive S3 object by tag
3) 3rd party tools & Gateways

# Upload to glacier

* Make use of description to persist metadata (in case local index is corrupted)
* Aggregate data into MBs , small data will have loads of overhead when persist into glacier
* Consider to persist file checksum with index locally ;
* Consider to persist file offset when files are aggregated, this helps to retrive data using range head
* Use multi-part upload


# Data Management

* Vault Tag
  * View billing by tag; config security by tag
* Integrate with CloudTrail
* Vault access policies: easy to control access and share content with other account
* __Vault Lock__ : 24hours cooling down / test period
* __Vault Access Policy__ : give more flexibility compare to Vault lock. For example, make use of the __Legal Hold__ tag on the vault

# Retrievals

Steps to retrieve data
* Step 1, request a job (which vault , which archive id, what range)
* Step 2, processing (depending type of request, can be minutes or hours)
* Step 3, Job completion notification
* Step 4, Download the data

Restore via S3 Lifecycle
* Request a file restore

3 Options:  Expedited --> Standard --> Bulk (cheapest, 12 hours for peta bytes)

# Reference

> Deep Dive Glacier
> https://youtu.be/l8ug62pVbtw
