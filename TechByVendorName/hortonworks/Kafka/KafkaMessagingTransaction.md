title: kafka message transaction
date: 2017-07-10 14:06:33
tags:
- hortonworks
- kafka
- transaction
---

https://cwiki.apache.org/confluence/display/KAFKA/Transactional+Messaging+in+Kafka


https://medium.com/@andrew_schofield/does-apache-kafka-do-acid-transactions-647b207f3d0e


So, does Apache Kafka do ACID transactions? Absolutely not. No way. Can you get a similar effect? If you design your applications in the right way, yes. Does it matter? In many cases, not really, but when it does, you absolutely don’t want to get it wrong. Just take the time to understand the guarantees that you need to make your system reliable and choose accordingly.

DB->Topic xa transaction, can do

Topic->DB xa transaction, hard to implmement
