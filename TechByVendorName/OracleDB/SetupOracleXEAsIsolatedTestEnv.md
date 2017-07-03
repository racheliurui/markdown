
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
