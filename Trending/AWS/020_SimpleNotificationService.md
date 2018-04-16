title: AWS - Simple Notification Service
date: 2018-04-16 11:03:23
tags:
- AWS
- SNS
---

# 061.mp4 062.mp4 -- overview

* Message being published to Topic via SDK/CLI/Console
* Subscribed by : SQS (FIFO Queue Not supported); Email (format: Email, Email-JSON); Mobile; HTTP(s); Lambda; SMS
* Message include,
   * MessageId, Timestamp, TopicArn, Type,UnsubscribeUrl, MessageBody, Subject,Signature, SignatureVersion
* SNS Mobile Push Notification Steps,
   * Request Credential from Mobile platforms
   * Request Token (ADM, GCM registration ID;APNS device Token)

* ADM(Amazon Device Messaging) -- push to kindle
* APNS --- push to iOS device
* GCM --- push to android


High-level Steps
https://docs.aws.amazon.com/sns/latest/dg/mobile-push-pseudo.html

Size limite for SMS (140byte per sms, 1600 byte per msg)
https://docs.aws.amazon.com/sns/latest/dg/sms_publish-to-phone.html

HTTP(s) protocole: user password
https://docs.aws.amazon.com/sns/latest/dg/SendMessageToHttp.html
