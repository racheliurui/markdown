title: Ambari UI integrate with AD
date: 2017-07-10 14:06:33
tags:
- hortonworks
- ambari
- security integration
---



```shell
[root@hdf-server01 ambari-server]# ambari-server setup-ldap
Using python  /usr/bin/python
Setting up LDAP properties...
Primary URL* {host:port}: ldapServer:389
Secondary URL {host:port} :
Use SSL* [true/false] (false):
User object class* (person):
User name attribute* (sAMAccountName):
Group object class* (group):
Group name attribute* (cn):
Group member attribute* (member):
Distinguished name attribute* ():CN=UserToPullUserData,OU=IT Department,DC=hortonworks
Base DN* :OU=IT Department,OU=IT hortonworks,DC=hortonworks
Referral method [follow/ignore] (ignore):
Bind anonymously* [true/false] (false):
Handling behavior for username collisions [convert/skip] for LDAP sync* (skip):
Manager DN* :CN=UserToPullUserData,OU=IT Department,DC=hortonworks
Base DN* :OU=IT Department,OU=IT hortonworks,DC=hortonworks
Enter Manager Password* :
Re-enter password:
====================
Review Settings
====================
authentication.ldap.managerDn: CN=UserToPullUserData,OU=IT Department,DC=hortonworks
Base DN* :OU=IT Department,OU=IT hortonworks,DC=hortonworks
authentication.ldap.managerPassword: *****
Save settings [y/n] (y)?
Saving...done
Ambari Server 'setup-ldap' completed successfully.

```

After setup , run below command to trigger the sync.
```shell
ambari restart
ambari-server sync-ldap --all
```
