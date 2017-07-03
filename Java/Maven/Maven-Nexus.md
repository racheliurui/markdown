
## Deploy rpm package to Nexus
### Method 1
```shell
mvn deploy:deploy-file \
    -DgroupId=com.github.diegopacheco.sandbox.devops \
    -DartifactId=fpmtest \
    -Dversion=1.0.0 \
    -DgeneratePom=true \
    -Dpackaging=rpm \
    -DrepositoryId=nexus \
    -Durl=http://127.0.0.1:8081/nexus/content/repositories/releases \
    -Dfile=slashbin-1.0-1.x86_64.rpm
```
### Method 2
```shell
curl -v -u admin:admin123 --upload-file slashbin-1.0-1.x86_64.rpm \
http://127.0.0.1:8081/nexus/content/repositories/releases/com/github/diegopacheco/sandbox/devops/fpmtest/1.0.1/fpmtest-1.0.1.rpm
```

### Method 3
```shell
curl -v -F r=releases -F hasPom=false -F e=rpm -F g=com.github.diegopacheco.sandbox.devops -F a=fpmtest -F v=2.0 -F p=rpm -F file=@slashbin-1.0-1.x86_64.rpm -u admin:admin123 http://127.0.0.1:8081/nexus/service/local/artifact/maven/content
```


# Reference links

https://maven.apache.org/guides/mini/guide-encryption.html

https://gist.github.com/diegopacheco/e04e90508451e8ce134b
