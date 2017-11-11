title: Flink
date: 2017-11-11 11:51:31
tags:
- bigdata
- data streaming
- flink
---

# setup the IDE

https://ci.apache.org/projects/flink/flink-docs-release-1.3/quickstart/java_api_quickstart.html


```shell
$ mvn archetype:generate                               \
      -DarchetypeGroupId=org.apache.flink              \
      -DarchetypeArtifactId=flink-quickstart-java      \
      -DarchetypeVersion=1.3.2
```


Providing the value of

```
Define value for property 'groupId': learn.rachel.flink
Define value for property 'artifactId': flinksamples
Define value for property 'version' 1.0-SNAPSHOT: : 0.1
Define value for property 'package' learn.rachel.flink: :
Confirm properties configuration:
groupId: learn.rachel.flink
```

This command should run under the folder where the source code parent folder not exist.
For example, run under D:\workspaces
Then, after the cmd run , the project root will be D:\workspaces\flinksamples
