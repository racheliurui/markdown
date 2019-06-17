title: Tibco -- FTL
date: 2018-07-02 18:19:00
tags:
- Tibco
- Tibco FTL
---


# Tibco FTL Community Edition

Default installation location on MAC.
The package was installed into /opt/tibco/ftl/5.4


# start a realm server

```
sudo /opt/tibco/ftl/current-version/bin/tibrealmserver
```

Visit the console at http://test.oms.onpretibcoftl1.sydney.gacims.net:8080


# start developing java client

```
# install client jar into maven
mvn install:install-file -Dfile=/opt/tibco/ftl/current-version/lib/tibftl.jar -DgroupId=tibco.ftl.client \
    -DartifactId=ftlclient -Dversion=5.4 -Dpackaging=jar

# For code using  com.tibco.ftl.group.*;
mvn install:install-file -Dfile=/opt/tibco/ftl/current-version/lib/tibftlgroup.jar -DgroupId=tibco.ftl.client \
    -DartifactId=ftlgroupclient -Dversion=5.4 -Dpackaging=jar

```

When running java , using -Djava.library.path=/opt/tibco/ftl/current-version/lib

# References

> Doc home
https://docs.tibco.com/pub/ftl/5.4.0/doc/html/GUID-C0EBAF36-A682-445D-B3EE-8E683330BE07-homepage.html
