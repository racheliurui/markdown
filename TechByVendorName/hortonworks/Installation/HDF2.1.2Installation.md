title: HDF 2.1.2 Cluster installation and configuration
date: 2017-05-05 10:10:33
tags:
- hortonworks
- hdf
- cluster
---

# HDF Cluster installation and configuration

## Overview
This wiki documented how to install and configure  Hortonworks HDF Cluster.

* HDF 2.1.2
 * Ambari 2.4.2.0
 * Kafka 0.10.1
 * NiFi 1.1.0
 * Ranger 0.6.2
 * Storm 1.0.2
 * ZooKeeper 3.4.6
 * MiNiFi Java Agent 0.1.0
 * MiNiFi C++ (Technical Preview)
* Linux OS level is RHEL 7 (centos7)

For more information about this version, refer to

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-release-notes/content/ch_hdf_relnotes.html

## Pre-requirements
### Network Requirement
#### host name
Make sure all the servers (ambari and the servers managed by Ambari) can ping each other by hostname.  
Use below command to check the full host name to all the servers to be configured.
```shell
hostname -A
```
#### network time service
Ambari Cluster rely on time service to sync important status.  

 * Option 1, if there's existing time service running, like chronyd ,  we should check the service is correctly configured.
  > If ntp service is not up and running, we may get warning during installation. It's safe to ignore the warning.

 * Option 2, Hortonworks recommends to use ntp service, if this service not started, we can manually install & configure & start it.
> yum install ntp
> service ntpd restart

####  DNS and NSCD

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-ambari-installation/content/check_dns.html

Follow above steps to prepare DNS and NSCD for all the hosts.

### Hardware requirements

Refer to below website for details of hardware requirements,

https://ambari.apache.org/1.2.1/installing-hadoop-using-ambari/content/ambari-chap1-2.html

### Software requirements

#### Standard tools need to check on every hosts

Refer below url for standard tools required by HDF cluster managed by Ambari.

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-ambari-installation/content/system-requirements.html

For RHEL/CentOS, we should check below tools.

* yum and rpm
* scp, curl, unzip, tar, and wget
*  openssl
 * v1.01, build 16 or later, user openssl version to check
*  python
 *  for CentOS 7 Python 2.7 (use python -V to check)

If there's warning, then check the Ambari Pkg version.

#### JDK need to check on every hosts

Recommended Oracle JDK can be installed following below command. Make sure the JCE package is included with the configured JDK.

```shell
subscription-manager repos --enable rhel-7-server-thirdparty-oracle-java-rpms
yum install java-1.8.0-oracle-devel.x86_64
```

Check the JDK path (/usr/lib/jvm/java), avoid including java version information in the path when used during ambari configuration.

### Set Up Password-less SSH (Optional)

__This step is optional , if we manually install Ambari-agent on managed hosts and start the agent , we can skip this step.__

Follow below Url to set up SSH tunnel between Ambari server to all the other managed servers.

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-ambari-installation/content/set_up_password-less_ssh.html

To verify the configuration, we should be able to ssh to other servers as root from Ambari server without typing password.

### Create Related Users (Optional)

For the benefit of enterprise linux server management,  we can manually create the users used by ambari cluster to give the users pre-assigned ids.
Here's an example of  hdf related users being added manually.

```shell
useradd -u 2050 -s /bin/bash ambari-qa
useradd -u 2051 -s /bin/bash ams
useradd -u 2052 -s /bin/bash atlas
useradd -u 2054 -s /bin/bash hbase
useradd -u 2056 -s /bin/bash hdfs
useradd -u 2058 -s /bin/bash infra-solr
useradd -u 2059 -s /bin/bash kafka
useradd -u 2060 -s /bin/bash kms
useradd -u 2064 -s /bin/bash nifi
useradd -u 2066 -s /bin/bash ranger
useradd -u 2073 -s /bin/bash zookeeper
useradd -u 2075 -s /bin/bash ambari
```



## Install Ambari Server and Agents

### Prepare Repositories
#### Option 1 use Satellite Server

If enterprise wide RHLS is using satellite server to manage the RPM repositories, we can follow below steps as an example to get ambari cluster installed.
* Step 1, check corresponding Hortonworks HDF Repository is hosted on satellite server
* Step 2, check to-be-configured servers subscribed Hortonworks HDF Repository.
* Step 3, check the Hortonworks HDF Repository is enabled on the server.

```shell
subscription-manager repos --disable bhpbio_Hortonworks_HDP_2_5_0_0_RHEL7 --disable bhpbio_Hortonworks_Ambari_2_4_0_1_RHEL7 --disable bhpbio_Hortonworks_HDF_2_0_0_0_RHEL7 --disable bhpbio_Hortonworks_HDP_2_5_3_0_RHEL7 --disable bhpbio_Hortonworks_Ambari_2_5_0_3_RHEL7 --disable bhpbio_Hortonworks_HDP_2_6_0_3_RHEL7
```

* verify the repository is prepared correctly on each server

```shell
subscription-manager repos --list-enabled | grep 'Repo ID'
```

#### Option 2 Local Repository

For environments without internet access and satellite servers, we can set up local repository using tar balls manually downloaded. We can host the repository as static contents over http.
> when using ambari server to configure cluster, the ambari-server will send command via ambari-agent to trigger Yum install on the managed server to get corresponding HDF components installed.  Make sure every server in the cluster has access to local repository.

Here's the urls used to download the repositories,

https://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-installation/content/ambari_repositories.html

https://docs.hortonworks.com/HDPDocuments/Ambari-2.4.2.0/bk_ambari-installation/content/hdp_stack_repositories.html

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-release-notes/content/ch_hdf_relnotes.html#hdf_repo


Here's an example of /etc/yum.repos.d/ambari.repo file pointing to local respository.

/etc/yum.repos.d/ambari.repo

```
#VERSION_NUMBER=2.4.2.0-136
[Updates-ambari-2.4.2.0]
name=ambari-2.4.2.0 - Updates
baseurl=http://<my-local-httpserver>/AMBARI-2.4.2.0/centos7/2.4.2.0-136
gpgcheck=0
gpgkey=http://public-repo-1.hortonworks.com/ambari/centos7/RPM-GPG-KEY/RPM-GPG-KEY-Jenkins
enabled=1
priority=1
```


###  Install ambari-server and ambari-agent
Checking the required repository correctly set for all the cluster hosts.

```shell
yum info ambari-server
yum info ambari-agent
```
 Run below command on ambari aerver host
```shell
yum install ambari-server.x86_64
```
Run below command on Ambari agent hosts.
```shell
yum install ambari-agent
```

##  Initial Ambari Server and Agent Setup

### Set up Amabari Server
#### Install HDF Management Pack

Ambari by default will run HDP management Pack, we should run the command to install HDF management Pack.

Refer to below Url for details of HDF management Pack downloading and installation (assuming our server do not have direct internet access).

https://public-repo-1.hortonworks.com/HDF/centos7/2.x/updates/2.1.2.0/tars/hdf_ambari_mp/hdf-ambari-mpack-2.1.2.0-10.tar.gz

https://docs.hortonworks.com/HDPDocuments/HDF2/HDF-2.1.2/bk_dataflow-ambari-installation/content/install-hdf-mpack.html

Once hdf-ambari-mpack-2.1.2.0-10.tar.gz  is manually downloaded, we can refer to it using file system location in the hdf management pack installation command.
```shell
ambari-server install-mpack \
--mpack=/local-file-location/hdf-ambari-mpack-2.1.2.0-10.tar.gz \
--purge \
--verbose
```

#### Ambari Server Initial setup and start

From ambari server , run below command to initial the server
```shell
ambari-server setup
```
* the JDK should point to the location (/usr/lib/jvm/java) we noted down at JDK preparation step

After the initial setup finished successfully, we should start the server.

```shell
ambari-server start
```

#### Ambari Agent Initial setup and start

This step is based on we will manually configure and ambari agent instead of the "password-less SSH" approach.
We need to modify the /etc/ambari-agent/conf/ambari-agent.ini config file, and make the agent pointing to Ambari Server we've setup.

```
[server]
hostname={your.ambari.server.hostname}
url_port=4080
```


After the initial configuration finished , we should start the agent.

```shell
ambari-agent start
```

## Create Ambari managed HDF Cluster

Log in Ambari console at, http://[ambari-server-host]:[default-8080] using default user admin @ admin.

Follow the Ambari Cluster creation wizard to create ambari cluster.

* For repository hosted locally,  put the correct url and check the option to validate the repository
* For repository hosted on satellite server, check the use satellite server option.
 *  There might be warnings when using satellite server, but as long as the repository hosted on satellite server is correctly configured, the warnings can be safely ignored.

### Cluster Configuration Wizard

While adding hosts following the cluster creation wizard, the automatic check against added servers will be triggered, the check result should be reviewed carefully.

We can manually run below command to clean up the agent servers.

```shell
python /usr/lib/python2.6/site-packages/ambari_agent/HostCleanup.py --silent
```

>Be careful above script will also delete manually configured Ambari repository config, if we are using local repository instead of satellite server, make sure required repository configuration is manually created after the scrip ( for example, manually created ambari.repo will be deleted and causing ambari metric management components failed to install during cluster configuration wizard).

### verify the Ambari Cluster

For a successful installation , there should be no errors.

## Enable Kerberos

After the cluster is created, we could choose to Kerberonize the cluster for better security.

Refer to below Url for details,

https://docs.hortonworks.com/HDPDocuments/Ambari-2.1.2.0/bk_Ambari_Security_Guide/content/_use_an_existing_active_directory_domain.html

Steps,

1) Apply to have an AD account which have full access to certain container.
2) Make sure the cluster is up and running with no issue.
3) Login the managed boxes, run kinit princialid to pre-authenticate the principal<
4) Login Ambari and enable Kerberos follow the wizard


## Possible Issues during installation and configuration

### cluster creation failure due to unsuccessful cleanup previous installation

```
resource_management.core.exceptions.ExecutionFailed: Execution of 'ambari-sudo.sh /usr/bin/hdf-select set all `ambari-python-wrap /usr/bin/hdf-select versions | grep ^2.1 | tail -1`' returned 1. symlink target /usr/hdf/current/zookeeper-client for zookeeper already exists and it is not a symlink.
```

This might to do with the /usr/hdf/current folder is generating symlink and those links are not cleaned up during re-installation.

https://community.hortonworks.com/questions/29153/ambari-install-failing-because-unable-to-obtain-hd.html

https://community.hortonworks.com/questions/1110/how-to-completely-remove-uninstall-ambari-and-hdp.html


__Resolve this issue we need to manually  remove all existing components installed and delete the content under /usr/hdf and then do a clean re-install on target server.__

### Configure to run Ambari-agent using non-root user

```
resource_management.core.exceptions.ExecutionFailed: Execution of 'ambari-python-wrap /usr/bin/conf-select set-conf-dir --package zookeeper --stack-version 2.1.2.0-10 --conf-version 0' returned 1. 2.1.2.0-10 Incorrect stack version
```

https://community.hortonworks.com/questions/60419/ambari-non-root-config-for-2401-incorrect.html
