title: AWS - Redshift Deepdive
date: 2018-07-29 17:41:23
tags:
- AWS
- Redshift
---


# Redshift Archi overview

![Redshift Cluster Archi ](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/023_RedShiftClusterArchi.png?raw=true)

* Bottom Layer: Ingestion Backup & Restore layer
* Leader Node & Compute Node
  * Leader node :
* Share Nothing MPP (Massive Parellel Processing) Architecture
* Reduce IO
  * Columnar Storage
  * Compress data  ( By Column)
  * __Zone Maps__ : in memory map about min and max value for given column in current block, to prune the query and reduce IO
* __Slices__
  * depending on cpu cores, each node support different number of slices
  * unit of data partitioning / parallel processing
  * table rows are distributed into different slices
* Data Distribution :
  * ALL; Key; Even(Round robin)
* Two types of hardwares as storage
  * HDD is slower but can scale to petabytes (2PB); SSD is faster but can only support to 300+ TB

## Storage Deep Dive

* Advertised (pricing) storage is 1/3 of the true utilized storage, because 2/3 used to data copies.
* __Blocks__ : column data persisted as 1MB immutable blocks.
    * With zone map metadata
    * location of next block
    * can be compressed
* Small write has similiar cost with larger write(1~10 rows = 100k rows)
* Update & Delete will only trigger soft delete, use VACUUM or DEEP COPY to delete ghost rows


# References
> https://youtu.be/iuQgZDs-W7A

# Overview

* <1k/TB/Year

## Data Ingestion

* Ingestion Source: SSH, S3, EMR, DynamoDB
* for COPY command, one slice can only single thread one COPY command.
  * To get 100M/s , you need multiple slices and multiple nodes
  * Batch inserts will save commit cost
  * If you have 16 slices, use 16 concurrent copy commands to 16 files to maximize performance
  * During COPY Redshift don't enforce primary key
  * Provide manifest file in json format on S3 while copying from S3 to make sure the load Behaviors are as expected.
* Redshift will appy Query Optimizer but how the optimize depends on statistics
  * COPY will do statistics automatically
* Redshift Data Compression
  * COPY will do compression automatically and select encoding automatially
* Data Hygiene
  * Analysis regularly (sort every week)
  * Vacuum regularly (weekly)
  * Use SVV_Table_Info
* Automatic Compression
  * Don't compress sort keys
    * If might result in you scan more rows than you needed ( many rows in one block by compression )
* Varchar column (define as small as possible)
  * the more varchar waste the memeory the less rows being loaded in memeory to do query (spilled into disk)

* Compound Sort Keys

* Don't Forklift

* On redshift :
  * Update = delete + insert
  * Commits are expensive ; blocks are immutable (1mb) -- load 1k rows a time
  * no small commit
  * Concurrency should be low for better throughput
* between redshift and dashboard, add a cache layer

* __Work Load Management__

## Security

* Source Data from S3 -- Use Envolope Encryption
* Encrypt data at rest
   * enable when create the cluster
   * Hardware acceleration (HSM)
   * ~20% performance impact
   * 4 layers of keys: block;database;cluster; master
     * Benefit: key rotate means use new key to encrypt the upper level key, not re-encrypt the whole data
* Encrypt data with certain column to restrict view to certain customer
* Support automatically encrypt Unload data (unload data from redshift to S3 files)

## UDF -- User Defined functions

* Use Python to write UDF
* Aggregate UDF
   * you need to implement ini function , aggregation function and finalize function

## Multi-demintional indexing with space filling curves

* When data started to grow, you started to have
  * __zone Maps__ : stores min max value of a block in memory
  * Sorting
  * Projection : mutiple copies of data sorted using different ways
* new keyword to index __INTERLEAVED__


## User reference

* automation framework : Azakaban (LinkedIn)


# Reference

> https://youtu.be/fmy3jCxUliM

> Deepdive 2014
> https://youtu.be/K-Usisr0zwg
