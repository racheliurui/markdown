title: AWS - Redshift Deepdive
date: 2018-07-29 17:41:23
tags:
- AWS
- Redshift
---

# Redshift Archi overview

![Redshift Cluster Archi ](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/023_RedShiftClusterArchi.png?raw=true)

* Ingestrion Backup & Restore layer
* Leader Node & Compute Node
* Share Nothing MPP (Massive Parellel Processing) Architecture
* Reduce IO
  * Columnar Storage
  * Compress data  ( By Column)
  * __Zone Maps__ : in memory map about min and max value for given column in current block, to prune the query and reduce IO
* __Slices__
  * one node has 2,16,32 slices
  * unit of data partitioning / parallel processing
  * table rows are distributed into different slices
* Data Distribution :
  * ALL; Key; Even(Round robin)

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

* for COPY command, one slice can only single thread one COPY command.
  * To get 100M/s , you need multiple slices and multiple nodes

* Data Hygiene
  * Analysis regularly (sort every week)
  * Vacuum regularly (weekly)
  * Use SVV_Table_Info
* Automatic Compression
  * Don't compress sort keys
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

## User reference

* automation framework : Azakaban (LinkedIn)


# Reference

> https://youtu.be/fmy3jCxUliM
