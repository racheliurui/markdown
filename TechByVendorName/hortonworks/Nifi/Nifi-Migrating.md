title: Nifi Data migration
date: 2017-07-10 11:06:33
tags:
- hortonworks
- nifi
- migration
---

# Nifi Data migration

Requirement, migrate existing Nifi flow into clustered environment without data loss.


https://community.hortonworks.com/questions/63745/migrating-nifi-flow-files-between-servers.html



# Nifi Dump


nifiDump.sh

```shell
# get 10 thread dumps
for i in {1..10}
do
  echo "start dump"
  /usr/hdf/current/nifi/bin/nifi.sh dump /tmp/nifi_Dump_$(date +"%Y_%m_%d_%I_%M_%p")
  echo "finished, sleep 60s"
  sleep 60s
done
```
