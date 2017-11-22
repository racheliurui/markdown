title: IIB connect to gmail
date: 2017-03-05 20:10:33
tags:
- ibm
- IIB
- pop3
---

# Prepare the Gmail Account for Pop3

* Gmail Settings :  Enable Pop3
* Enable Less secure apps

 https://myaccount.google.com/lesssecureapps

# Prepare the TrustStore for IIB


https://www.avisi.nl/blog/2012/09/12/quick-way-to-retrieve-a-chain-of-ssl-certificates-from-a-server/

To retrieve the certificate from gmail pop server.

```shell
openssl s_client -host pop.gmail.com -port 995 -prexit -showcerts

```

Then save each of the certs as separate .cer or .pem file and import into the truststore.


# Set up IIB run time env

```cmd
mqsisetdbparms TESTNODE_Rachel -n gmailtestaccount -u myemailaddress@gmail.com -p mygmailpassword

mqsisetdbparms TESTNODE_Rachel -n email::gmailtestaccount -u myemailaddress@gmail.com -p mygmailpassword
```

```cmd
mqsichangeproperties TESTNODE_Rachel -e default -o ComIbmJVMManager -n truststoreFile -v "D:\temp\certs\localhost.truststore.jks"
mqsisetdbparms TESTNODE_Rachel -n brokerTruststore::password -u temp -p mytruststorepassword
mqsichangeproperties TESTNODE_Rachel -e default -o ComIbmJVMManager -n truststorePass -v "brokerTruststore::password"
mqsichangeproperties TESTNODE_Rachel -e default -o ComIbmJVMManager -n truststoreType -v JKS
mqsichangeproperties TESTNODE_Rachel -e default -o ComIbmJVMManager -n jvmSystemProperty -v "-Dmail.smtp.auth.enable=true -Dmail.smtp.starttls.enable=true"
```

to check the current configuration
```cmd
mqsireportproperties TESTNODE_Rachel -o BrokerRegistry -a
mqsireportproperties TESTNODE_Rachel -e default -o ComIbmJVMManager -a
```


# Configure the IIB Email Input node

Email Server: pop3s://pop.gmail.com:995
Security Identity: gmailtestaccount


# Reference Links

https://developer.ibm.com/answers/questions/210834/how-configure-email-input-node-to-receive-emails-w.html
