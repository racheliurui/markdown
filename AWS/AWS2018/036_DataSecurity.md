title: AWS - Data Security
date: 2018-04-23 10:16:00
tags:
- AWS
- Data Security
---

# Encryption and Key Management in AWS

> Encryption and key management in AWS --2015
> https://youtu.be/uhXalpNzPU4

## Encryption Primer

* Encrypt the data using __Symetric key__ and store the encrypted data
* Use __Master Key__ to encrypt __Symetric Key__ and store the encrypted key
* Use __Master master key__ to encrypt master key, and store the encrypted master key
* ........, these keys are called __Key Hierarchy__, and store in HSM
   * Reduce the __Blast Radius__ about losting a single key

## Client Side Encryption

* customer encrypt the data and manage key
* For client side encrypted data targeting to store in S3, you can use AWS SDK to simplify the approach
  * Using AWS SDK, your master key will be on premise, but symetric key and encrypted data will saved in S3

## Server Side Encryption

* AWS encrypt the data and manage key
  * upload raw data via TLS to AWS (S3, Glacier, EBS, Redshift, RDS etc), then enable encryption
    * Use AWS key, AWS will generate dynamic unique key for each object, then manage the key using aws s3 internal service
    * Use customer key, AWS will encrypt the data using customer's key and throw away the key after encryption
       * when request the encrypted data, you need to provide the key , aws decrypt the data and return it back
* For S3/EBS/RDS/Redshift server side encryption, you have 2 options,
  * use S3/EBS/RDS/Redshift service master key (who ever have access to bucket will be able to decrypt the data)
  * use AWS KMS service, then you can specify which master key you want to use when encrypt the data



## Key management Options

* self manage
* AWS Key Management Service
  * Use API to generate the key, encrypted and plain text key will be returned
    * Plaintext is used to encrypt the data, encrypted key is stored locally
    * when decryption needed, client need to submit the locally stored encrypted key
    * Master key is alwasy stored in KMS.
    * Benefit: KMS have more fine-grained access control, so encrypted data can only be decrypted by user who have access to the key.
    * Better auditing
    * Plain text never exist in any persist storage; AWS Service operate team is fully separated with KMS team; Multi-Party control
* AWS Partner solutions
    * Browse AWS marketplace for security
* AWS CloudHSM -- HSM is hardware Security module
    * A box used to store the keys
    * Only the user have access to the module
    * You can use offical cloudformation to provision it
    * support oracle/sql server( run on EC2 ) encrytion with HSM
    * support EBS storage encryption
    * support redshift (the only one )
* KMS vs HSM
  * KMS underlyingly using HSM platform but not dedicated HSM
  * HSM is useful to comply government standards

# 102.mp4 103.mp4 -- Data Security


* __Distributed DDOS attacks__
* __Man In the Middle (MITM) attacks__ : 截获并替换证书，从而在中间修改消息内容。（如何防范： 避免信任self-signed证书）
* __IP Spoofing__ : IP 地址欺骗 (???)
* __Packet Sniffing__ : like what wiredshark can do




==========================
X.509 certificate

HMAC-SHA256 protocol

cloud HSM??

IPsec VPN
