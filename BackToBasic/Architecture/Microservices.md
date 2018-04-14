title: microservices architecture reading
date: 2017-04-12 10:10:33
tags:
- architecutre
- Microservices
---

https://www.youtube.com/watch?v=CZ3wIuvmHeM


Hystrix --
FIT (Fault Injection Testing)
Critical Microservices --- increase the availability
Client Libraries --- simplified lib


CAP Theorem: In the presence of a network partition, you must choose between consistency and availability
Netflix's solution: Eventually Consistency (tech stack: Cassandra)

Infrastructure
multi-region strategy

Stateless service
 * Not a cache or database
 * Frequently access metadata
 * No instance affinity
 * Loss a node is a non-event

Stateful service
 * database & caches
 * custom apps hold large amounts of data
 * Loss of a node is a notable event


EVCache
  * separate the write to different available zones
  * read from local zone

improve EVCache: separate different requests

Spinnaker  


Conway's Law
Organizations which design systems are constrained to produce designs which are copies of the communication structures of these organizations.
Any piece of software reflects the organizational structure that produced it.
