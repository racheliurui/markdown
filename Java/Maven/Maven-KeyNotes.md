title: Maven - Refresh
date: 2018-06-25 19:31:23
tags:
- Maven
- DevOps
---

# Key Terminology

https://maven.apache.org/guides/

* LifeCycles

* Project Inheritence vs Aggregation vs Mixed

> https://maven.apache.org/guides/introduction/introduction-to-the-pom.html

* Build profile
  * per Project
  * per user
  * Global
  * Profile Descriptor (dynamically loaded in project)
> https://maven.apache.org/guides/introduction/introduction-to-profiles.html

```cmd
# Maven Command

# mvn groupId:artifactId:goal -Denvironment=test -P profile1,!profile2

mvn com.mycom.app1:module1:deploy -Denvironment=test -P profile1, !profile2
mvn help:active-profiles -Denv=dev
mvn help:effective-pom -P appserverConfig-dev

```


# Maven with Docker

Hands on to build a maven java project as a docker image
https://examples.javacodegeeks.com/devops/docker/introduction-docker-java-developers/
