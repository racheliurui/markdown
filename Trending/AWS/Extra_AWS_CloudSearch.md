title: AWS - CloudSearch
date: 2018-07-02 19:38:23
tags:
- AWS
- CloudSearch
---


# AWS CloudSearch

## Overview

Key difference between db query and search engine:
DB query will give you exact result, search engine only gives you best result.

## DeepDive

* Core based on Apache Solr
* Key info with deepdive
  * setting up
  * Scaling
  * Queyring
  * Architecting

## Setting up

* Create and config a domain ( cli )
* Create batches ( go to data store, change data format to Solr supported type )
  * Use maximum batches (5M bytes) -- use max sized batches
  * conver data: remove bad charactors

* integrate with IAM (who can connect to which domain)
* integrate with cloudtrail

## Scaling

* tip: increase instance type for load-in
   * Test against different type of data (tweets vs web data)
   * Options will have effect on index size
   * muti-threads to upload data ( test the limites to avoid 500 error)
* set multi partition
* Pre-warm for traffic spike

## Query

* Rest api
* Geo Filtering
* fq ( used to filter result) vs q
* Geo sorting ( sort by distance)
* Boosting (certain key word has higher ranking)
* Multi-language supported.
* AWS SDK

## Architecting

* Cache , add elastic cache before cloud engine
* Consider Muti tenant
  * Option1,
    * feed all data into same domain
    * use a field with customer id
  * Option2,
    * multi domain
  * how to choose:
     * if each customer has very different config, then use multi domain
     * if each customer has similiar config, but we have a lot of different customers, then set up a lot of domain is not cost effective.
* Mine user behavior to improve result ( user search result log into EMR , analysis result feed back in as search parameters to help imporve search result)
     * help with document boosting; augmentation; synonym creation

# Reference

> AWS CloudSearch DeepDive and Best Practices
https://youtu.be/OeHaj1a66I4
