title: Tibco -- BusinessWorks
date: 2018-07-20 10:19:00
tags:
- Tibco
- BusinessWorks
---

Try to create a docker image -- used to build ear.

```docker
# use same version of ubuntu
# simulate the prod environment
FROM ubuntu:latest
COPY ./TIB_BW_6.4.2_linux26gl23_x86_64.zip /installtb
COPY ./TIB_bwpluginftl_6.4.1_linux26gl23_x86_64.zip /installplugin

CMD cd /installtb
CMD unzip TIB_BW_6.4.2_linux26gl23_x86_64.zip
CMD ./TIBCOUniversalInstaller-lnx-x86-64.bin -silent -V responseFile='TIBCOUniversalInstaller_BW_6.4.2.silent'

CMD cd /installplugin
CMD unzip TIB_bwpluginftl_6.4.1_linux26gl23_x86_64.zip
CMD ./TIBCOUniversalInstaller-lnx-x86-64.bin -silent -V responseFile='TIBCOUniversalInstaller_bwpluginftl_6.4.1.silent'

CMD rm -rf /installplugin
CMD rm -rf /installtb

```
docker build -t liuruibnu/bw641:v1 .
docker run -it liuruibnu/bw641:v1 ls /opt/tibco
docker run -it liuruibnu/bw641:v1 ls ~/.TIBCO/
