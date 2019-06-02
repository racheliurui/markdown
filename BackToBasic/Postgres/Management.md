title: PostgreSQL
date: 2019-06-01 20:36:30
tags:
- PostgreSQL
- basic
---


# Reference

https://aws.amazon.com/blogs/database/managing-postgresql-users-and-roles/


CREATE ROLE readwrite;
GRANT CONNECT ON DATABASE "Datawarehouse" TO readwrite;
GRANT USAGE ON SCHEMA "dw_cons" TO readwrite;
GRANT USAGE, CREATE ON SCHEMA "dw_cons" TO readwrite;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA "dw_cons" TO readwrite;
ALTER DEFAULT PRIVILEGES IN SCHEMA "dw_cons" GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO readwrite;
GRANT USAGE ON ALL SEQUENCES IN SCHEMA "dw_cons" TO readwrite;
ALTER DEFAULT PRIVILEGES IN SCHEMA "dw_cons" GRANT USAGE ON SEQUENCES TO readwrite;

GRANT readonly TO "tableau_read";
GRANT readwrite TO "tibco_write";
