title: AWS - KMS
date: 2018-06-09 16:02:00
tags:
- AWS
- AWS KMS
---

# Background

Application Security Design goals

__CIA__ (Confidentiality, Integrity, Availability)
  * __Confidentiality__:
     * AWS is using the __PARC__ Model
        * Principal, Action, Resource, Condition
  * __Integrity__
  * __Availability__: how long to encrypt/decrept the data, and how long the customer can stand if any of the system is not available and needs failover
     * For example, how long it take to encrypt the data and write to S3

# Key Implement to meet CIA requirements

1) "Don't store secret as plaintext on disk" and "Decrypt only happens in your instance"
  * means encrypt and decrypt only happens inside your code inside your instance. (not aws service side)
  * User AWS KMS client SDK; S3 encryption client ; DynamoDB encryption client
  * __Envolop Encryption__ : use random key to encrypt each piece of data, encrypted data and corresponding key stored together, the key will be encrypted using master key before being stored.
2) "keep cipher text of secret in multiple locations"
  * make use of S3 --> 11 9s durability or DynamoDB (if you consider latency)
3) "Make sure secrets not being changed since last used"
4) "if instance can launch, secret should be accessible; <1 min to provision plaintext secret to instance "
  * KMS exist in every Region (except China :( ) ;
  * Make careful decisions between retriving each time or caching in memory

* Key policy is the king ! Key policy not equals with IAM policy.



# Case Study : Okta

Okta is a unit of measure for cloud cover. From 0 to 8 describe how much visability it is.

* Simple Best Priatice to
  * Data from Database needs to be encrypted at rest or in memory
  * Encrypt Key only in Memory
  * Service has access to plain text data

![Okta Encrypto Mode](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/Extra_KMS_Okta_mode.png?raw=true)


# References

>https://youtu.be/EgJ9EQn0EJ8

>https://youtu.be/EgJ9EQn0EJ8
