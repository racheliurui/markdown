title: Linux commands
date: 2017-05-12 18:38:33
tags:
- basic
- Security
- Symmetric vs Asymmetric
- X509
---

* Symmetric vs Asymmetric encryption

https://www.ssl2buy.com/wiki/symmetric-vs-asymmetric-encryption-what-are-differences

* 对称密钥的意思就是加密解密用一样的密钥
* 非对称加密就是公钥私钥交叉加密的方式

# x.509 certification

An X.509 certificate contains a public key and an identity (a hostname, or an organization, or an individual), and is either signed by a certificate authority or self-signed.

The structure of an X.509 v3 digital certificate is as follows:

Certificate
Version Number
Serial Number
Signature Algorithm ID
Issuer Name
Validity period
Not Before
Not After
Subject name
Subject Public Key Info
Public Key Algorithm
Subject Public Key
Issuer Unique Identifier (optional)
Subject Unique Identifier (optional)
Extensions (optional)
...
Certificate Signature Algorithm
Certificate Signature

## Certificate filename extensions
There are several commonly used filename extensions for X.509 certificates. Unfortunately, some of these extensions are also used for other data such as private keys.

* .pem – (Privacy-enhanced Electronic Mail) Base64 encoded DER certificate, enclosed between "-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----"
* .cer, .crt, .der – usually in binary DER form, but Base64-encoded certificates are common too (see .pem above)
* .p7b, .p7c – PKCS#7 SignedData structure without data, just certificate(s) or CRL(s)
* .p12 – PKCS#12, may contain certificate(s) (public) and private keys (password protected)
* .pfx – PFX, predecessor of PKCS#12 (usually contains data in PKCS#12 format, e.g., with PFX files generated in IIS)
