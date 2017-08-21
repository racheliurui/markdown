title: Reset Ranger Password
date: 2017-07-10 14:06:33
tags:
- hortonworks
- ranger
---


# forgot the username and password

https://community.hortonworks.com/content/supportkb/49508/how-to-change-grafana-admin-password-when-the-pass.html


# Ranger UI password missing after switching the UI authentication

https://community.hortonworks.com/questions/4408/is-there-any-way-to-reset-ranger-admin-ui-password.html

```shell
vi /var/lib/pgsql/data/pg_hba.conf
```

add below line to give access,
```
local all angerdba trust
```

```shell
psql rangerdb -U rangerdba
```

```shell
update x_portal_user set password = 'ceb4f32325eda6142bd65215f4c0f371' where login_id = 'admin';
```
