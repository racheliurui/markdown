title: Make use of the open sourced wrapper
date: 2017-04-15 16:10:33
tags:
- hortonworks
- nifi
---

# Make use of the open sourced wrapper

https://github.com/kohsuke/winsw/blob/master/doc/installation.md

# Prepare Nifi windows server


1.  Make sure JDK  1.8 above is installed on the windows server and in the class path running the service. If not, change the bootstrap.properties to point to correct JDK.

 __At current stage, JRE is not tested__     

2. Here's an example of config nifi.xml ,put the file under nifi root folder,

 ```xml
<service>
     <id>nifi</id>
     <name>nifi</name>
     <description>this service runs nifi solution</description>
     <env name="APP_HOME" value="%BASE%"/>
     <logpath>%BASE%\logs</logpath>
     <logmode>rotate</logmode>       
     <executable>%BASE%/jre1.8.0_121/bin/java.exe</executable>
        <arguments>-cp %BASE%\conf;%BASE%\lib\bootstrap\* -Xms12m -Xmx24m -Dorg.apache.nifi.bootstrap.config.log.dir=%BASE%\logs -Dorg.apache.nifi.bootstrap.config.pid.dir=%BASE%\run       -Dorg.apache.nifi.bootstrap.config.file=%BASE%\conf\bootstrap.conf org.apache.nifi.bootstrap.RunNiFi Start</arguments>
</service>
```

3. Put the winsw-2.1.0-bin.exe under the folder of nifi root folder, change the name to nifi.exe

4. Use windows shell command line , run nifi.exe install to install the service

 ```shell
nifi.exe install
```
