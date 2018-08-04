title: Docker command cheet sheet
date: 2018-07-08 11:13:23
tags:
- docker
- Cheetsheet
---



# build a image


```shell
docker build -t account/repo:tag .
#  specify the build file
docker build -t account/repo:tag --file dockerfilename .

#debug generated image


# create a volumn
$ docker run -it -v "$PWD":/usr/src/mymaven -v "$HOME/.m2":/root/.m2 -v "$PWD/target:/usr/src/mymaven/target" -w /usr/src/mymaven maven mvn clean package  

docker volume create --name workspace

docker run -it -v workspace:/workspace maven mvn archetype:generate # will download artifacts
docker run -it -v workspace:/workspace maven mvn archetype:generate # will reuse downloaded artifacts
docker run etpartner/tibco:bw642ftl ls /opt/tibco/bw/bw/6.4/bin/bwdesign


docker cloud
creat repo ; then
docker login
docker push
```
