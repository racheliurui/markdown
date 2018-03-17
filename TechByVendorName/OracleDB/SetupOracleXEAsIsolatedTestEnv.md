title: notes of setting up Oracle DB for test env
date: 2017-02-11 20:10:33
tags:
- oracle
- devops
- docker
---

# For existing DB, build isolated test from scratch

## install oracle XE

* connect to system instance
```
connect system/password;
```

the password that you entered during the installation.
* create user

create user username identified by 'password';
and also to give this user some privileges for creating tables, views and so on . .

* grant access

```
grant dba,resource, connect to username;
GRANT Create Session TO username;
```

## Obtain the DDL of the table needed

In dev Oracle DB

1) Install Oracle XE


1) using sql developer to export existing schema definition

2) in new database,
create required table space
create tablespace tablespacename datafile 'datafilename.dbf' size 40m online;
change tablespace's autoextend pram
ALTER DATABASE DATAFILE 'datafilename.dbf' AUTOEXTEND ON MAXSIZE UNLIMITED;


1) in SQL developer, connect to database using created user
Export existing schema definition using SQL Developer
Run in backuped database

1) create same schema by create the user
    create user <schema-name> identified by <schema-name>;


# Issues

``` xml
<ErrorDesc>Child SQL exception ( HY000 2289 [IBM][ODBC Oracle Wire Protocol driver][Oracle]ORA-02289: sequence does not exist )</ErrorDesc>
```

__Reason__

forgot to create trigger for the tables.



## set up Oracle with Docker on Mac OS

https://www.esentri.com/blog/2017/05/15/create-and-use-a-docker-container-with-oracle-xe-on-macos/
https://github.com/oracle/docker-images/tree/master/OracleDatabase

```
./buildDockerImage.sh -v 12.2.0.1 -s -i
docker images
docker run  \
-p 1521:1521 -p 5500:5500 \
-e ORACLE_SID=ORCLCDB \
-e ORACLE_PDB=ORCLPDB1 \
-e ORACLE_CHARACTERSET=AL32UTF8 \
-v /Users/ruiliu/data/oracledata:/opt/oracle/oradata \
oracle/database:12.2.0.1-se2

docker ps -a
docker exec 35d6ef419dba ./setPassword.sh eVG7PQ0DnxcI
```


```log
ORACLE PASSWORD FOR SYS, SYSTEM AND PDBADMIN: eVG7PQ0Dnxc=1

LSNRCTL for Linux: Version 12.2.0.1.0 - Production on 14-FEB-2018 12:17:23

Copyright (c) 1991, 2016, Oracle.  All rights reserved.

Starting /opt/oracle/product/12.2.0.1/dbhome_1/bin/tnslsnr: please wait...

TNSLSNR for Linux: Version 12.2.0.1.0 - Production
System parameter file is /opt/oracle/product/12.2.0.1/dbhome_1/network/admin/listener.ora
Log messages written to /opt/oracle/diag/tnslsnr/35d6ef419dba/listener/alert/log.xml
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1)))
Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=0.0.0.0)(PORT=1521)))

Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1)))
```

```
docker container ps  -a
docker start  35d6ef419dba
docker exec -ti 35d6ef419dba sqlplus pdbadmin@ORCLPDB1

docker stop 35d6ef419dba
```

# connect from SQL Developer

user pdbadmin
password eVG7PQ0DnxcI
port 1521
service name ORCLPDB1

data storage: ~/data/oracledata

# shutdown the environment

```
docker stop 35d6ef419dba
docker container ps  -a
```
