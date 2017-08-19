title: Linux commands
date: 2017-04-15 16:10:33
tags:
- linux
- basic
- shell
---

# for Redhat Linux

* list all the repositories


# Common Linux

* find current folder size
```
findmnt /tmp -o SOURCE,FSTYPE,SIZE,USED,AVAIL,USE%,TARGET
findmnt /var -o AVAIL
```

* check current user group

 ```
$ groups username
$ usermod -a -G groupname username
```

* check RAM and CPU information

```
 cat /proc/cpuinfo
 ```

## subscription-manager

```shell
subscription-manager repos --disable '*Hortonworks*'
subscription-manager repos --enable Hortonworks_Ambari_2_5_1_0_RHEL7 --enable Hortonworks_Ambari_2_4_2_0_RHEL7 --enable Hortonworks_HDP-UTILS_1_1_0_21_RHEL7
#check existing package included in certain repo
yum list available | grep Hortonworks_Ambari_2_5_1_0_RHEL7
```

* find port 7000 is used by which process

```shell
# fuser 7000/tcp
```
