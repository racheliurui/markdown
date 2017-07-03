
# Setup a spring boot application to run on windows as a service

# Reference links
https://docs.spring.io/spring-boot/docs/current/reference/html/deployment-install.html

https://github.com/kohsuke/winsw/blob/master/doc/installation.md

https://github.com/snicoll-scratches/spring-boot-daemon

Basically, winsw provide a way to make executable to run as a windows service.

Tips,
1. Check winsw log for the real command being run
2. Security (run as certain user from the service)


A basic example,

```xml
<service>
	<id>myapp</id>
	<name>myapp</name>
	<description>this service runs myapp solution</description>
	<env name="APP_HOME" value="%BASE%"/>
	<logpath>%BASE%\logs</logpath>
	<logmode>rotate</logmode>

	<executable>%BASE%/jre1.8.0_121/bin/java.exe</executable>
        <arguments>-Xmx256m -jar %BASE%\myapp.jar -Dlog4j.configuration=%BASE%\config\log4j2.xml --loader.path=%BASE%\config --spring.config.location=%BASE%\config\myapp.properties</arguments>
</service>
```

# externalize spring config file

https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html
for example,

--spring.config.location=/opt/myconfig.properties
