title: notes of setting up mysql for test env
date: 2017-02-11 20:10:33
tags:
- mysql
- devops
---


# On windows

## start and trigger mysql cmd
* start default mysql db engine

 ```shell
mysqld
```

* connect to mysql engine and get the cmd prompt
```shell
mysql -u root -p -h localhost
```

## cmd run under mysql cmd prompt

* list database
 ```shell
show databases;
```
* create new database
 ```shell
create database poc;
 ```
* switch database
 ```shell
use poc;
 ```
* list tables
 ```shell
show tables;
 ```
