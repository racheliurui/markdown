title: JDK or JRE?
date: 2017-11-18 13:42:31
tags:
- JDK
- security
- basic
---

https://serverfault.com/questions/372997/why-jdk-is-installed-with-web-application-servers

# What's the differnce between using JDK and JRE?

The requirement of JDK or JRE is dependent on the particular application server itself. (e.g JBOSS, tomcat, glassfish, etc), and its strategies for compiling to bytecode, and how it decides on its dependencies at start-up.

In a strict sense if your java application just executes Java byte code in the form of classes, then you should be able to get away with just a JRE. However whether this is true or not depends on the Java App server strategy to either check for an installed JDK defensively at start-up, or just throw an exception at some point when compilation is requested.

Some application servers use the javac to compile jsp to class files and hence are dependent on having a system JDK installed, this can be contrasted with say tomcat, which bundles its own compiler for jsps, hence can run under the JRE.

The java keystore is a feature of the Java SE, and both openJDK and Hotspot reference a file $JAVA_HOME/lib/security/java.security to select their defaults.

Unless you have changed $JAVA_HOME/lib/security/java.security, the default keystore.type=jks file implementation looks for $HOME/.keystore hence its up to you to over ride the location, and both the 1.5 and 1.6 version of the sunJDK use that format and default location.

so basically changing $JAVA_HOME wont effect the location of the keystore

(unless you have actually over ridden the keystore location into the $JAVA_HOME folder...)

but it might matter if you are using some non-default provider, or have set some non-default options in java.security.
