title: AWS - WAF and Shield
date: 2018-06-01 11:49:00
tags:
- AWS
- AWS WAF
- AWS Shield
---


# Summary


| Threats | DDoS | Application Attacks | Bad Bots|
|---|---|---|---|
|Application Layer| HttpFloods <<-- Shield Advanced| SQL Injection; Sensitive Data Explosure; Social Engineering; Application exploits  <<-- WAF | Crawler; Content Scraper; Scanner&Probe <<--WAF|
|Network Layer| Reflection; SSLAbuse; Amplification; SlowLoris; Layer4Floods <<-- Shield Standard ||


# DDOS

 * Layer 3/4 DDoS
   * SYN/UDP Floods
   * reflection Attacks
* Layer 7 DDoS

# Key Features

## AWS Shield

* Standard : layer 3/4 protection
* Advanced : layer 7 protection
  * with AWS Shield, WAF is free
  * DDoS Scaling up free (report and refund)
  * Available when you have App ELB, Classic ELB, CloudFront, S3

## AWS WAF

Feature Summary,

* Filter traffic based on customized rules
* Malicious Request protection
  * SQL injection
  * Process encrpting (???)
* Active monitoring and tuning


* Less than 55 sec before the new rule is applied globally
* Less than 1 minisec inspection time when turned on
* API & SDK support when define the rules
* Pre-configured rules

How to use,
* Flexible customized Rules
* Pre-configured rules
* Security Automations (combines with Lambda;)

Common use case for WAF,

* IP Reputation List
  * Can deploy this feature using cloudformation
  * update reputation list from 3 trustful sources
* HTTP floods
  * limit number of http requests per client in a 5 min bucket
* Scanners & probes
  * available to deploy using cloudformation
  * Honney pot url (???)

### DEMO-1 WhiteList good user

![WAF config](https://github.com/racheliurui/markdown/blob/master/Trending/AWS/images/Extra_WAF_WAFConfig.png?raw=true)
Define conditions --> attach condition to rules --> attach rules to WEB ACLs, associate WEB ACLs to AWS services (S3, CloudFront, ELB)

### DEMO-2 Virtual Patching
Example : Apache Struts Vulnerability
When a condition is attached to Rule, you can define whether block or allow when the condition is true

Rate-Based Rule + URI String Match Condition = protect Brute Force Login Attemps


### DEMO-3 Brute Force on Login

When define a rule , there are 2 options,  "Regular rule" or "Rate Based Rule". For this scenario, we use "Rate Based Rule"

Define "Rate Based Rule" with the "/login" URI match condition, set 2000 times / 5 min

# OWASP Top 10

* A1 : Injection

* A2 : Broken Authentication and Session Management
  * Hard to distinguish legistimate Users
  * Automate update of black list of token When
    * different location with same token
    * abnormal login rate

* A3 : Cross Site Scripting (XSS)
  * for example, a blog platform has a user published a blog with embeded script loading from his own website to run in browser (who ever browse this perticular blog) and exploit the key inputs
  * It's easy to block content with Script tag from Body, querystring or cookie; but needs to be carefully thinking about other requirement like SVG graphics (using script tag)

* A4 : Broken Access Control

  * http://mywebsite/editprofile?userid=1234 ; once authenticated, user 1234 can access http://mywebsite/editeprofile?userid=4567
    * __Mitigate__: Hard, possibly match signature
  * http://mywebsite/download?file=file1.pdf ; once authenticated, user can manipulate the file path and expose any file on Server (http://mywebsite/download?file=../../../../etc/passwd)
    * Directory Traversal
    * Local file Inclusion
    * __Mitigate__:can use WAF to match against ../,://
  * http://mywebsite/?module=myprofile&action=edit ; once authenticated, user try to visit http://mywebsite/?module=management&action=edit
    * __Mitigate__: limit source if possible;
    * __Mitigate__: User lambda@edge (???)
* A5 : Security Misconfiguration
    * Leave web server __ServerTokens Full__ (default config) which expose exact version and components for attackers to use known Vulnerabilities
    * Leave default directory listing enabled
    * Return stack trace in error page
    * PHP bug to allow request parameter registered as global variable; attackers use this to overwrite global variable http://mywebsite/?_SERVER[DOCUMENT_ROOT]=http://attackerswebsite/bad.htm ; this will change doc root to another website.
      * __Mitigate__: block query string with _SERVER
    * __Mitigate__: use __Amazon Inspector__ check against common known mis-configurations
    * __Mitigate__: User __AWS Config__ and __EC2 System Manager__ to track configuration changes over time.
* A6 : Sensitive Data Explosure
  * SHA-1 hashing algorithm; attackers can attempt to cause __hash collision__
  * __Mitigate__: Both ELB and Cloudfront support specify allowed ciphers
* A7 : Insufficient Attack Protection
  * Submit abnormal huge number of requests or single request with huge payload
  * __Mitigate__: Rate based rules & size constraint Rules
  * __Mitigate__: __WAF Security Automation__ with Lambda
    * Lambda analysis access log to update block ip
    * update block ip list from reputation list
    * Honeypot URL
* A8 : Cross Site Request Forgery (CSRF)
  * Different with XSS. This is relying on user's trust to browser
  *  embed this link <img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">; user who click it & just logged in online banking will transfer $1000 to Badman
   * __Mitigate__: embeded hidden token(GUID) in form or header.
   * __Mitigate__:  check refer header is from correct source ( won't work if the browser implementation is changed)
* A9 : Using components with known vulnarables
  * CVE: common Vulnerabilities and Exposures
  * __Mitigate__:Filter out components not being used in your application
  * __Mitigate__: __Penetrating Test__ ; needs aws permission
* A10 : Unprotected APIs
  * same with A1-A9 but with API

* Old A10 : Unvalidated Re-directs and Forwards
  * Url shorten solution and user generated a shoren url using original one like : http://mysite/link?target=http://badsite
  * __Mitigate__: white list to target url or uri

## OWASP TOP 10 Cloudformation Templates

> https://github.com/aws-samples/aws-waf-sample/blob/master/waf-owasp-top-10/owasp_10_base.yml

# References

> https://youtu.be/W01f7g7slHw

Use WAF to mitigate OWASP TOP 10 Coverage
https://d0.awsstatic.com/whitepapers/Security/aws-waf-owasp.pdf


SHA 1  and hash collision
> https://zh.wikipedia.org/wiki/SHA-1
