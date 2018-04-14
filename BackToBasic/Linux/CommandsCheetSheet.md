title: Linux commands
date: 2017-04-15 16:10:33
tags:
- linux
- basic
- shell
---

# for Redhat Linux

* list all the repositories



* find current folder size
findmnt /tmp -o SOURCE,FSTYPE,SIZE,USED,AVAIL,USE%,TARGET


* centos minimal might not contains netstat

yum install net-tools

https://cyruslab.net/2014/07/11/installing-netstat-on-centos-7-minimal-installation/



# list block device

```shell
lsblk
```
https://www.centos.org/docs/5/html/Deployment_Guide-en-US/ch-lvm.html

Type:
disk
part
lvm
rom
