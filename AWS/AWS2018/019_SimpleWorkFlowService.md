title: AWS - Simple WorkFlow Service
date: 2018-04-16 10:37:23
tags:
- AWS
- SWF
---

# 059.mp4 060.mp4 - SWF overview

SWF: __Simple WorkFlow Service__

* long running process
* interact with aws, user, on-promise infrastructure
* WorkFlow Engine
* Workflow and sub WorkFlow
* SWF Domain : one or multiple workflows
* Actor:
   * Starter
   * Decider
   * Worker
* Task:
   * Register via console or CLI (RegisterActivityType)
   * Specify Queue for task
   * Use "Task Routing" for routing to specific worker
* Implementation / Set up
   * Implementation : SDK; API Call ; framework (Java / Ruby)
   * Setup : CLI or console
* A scenario :
   * worker upload video --> transform --> review --> online


Steps to develop and run a WorkFlow
https://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-dg-intro-to-swf.html

SWF limitations (number of domains; request size; flow execution time)
https://docs.aws.amazon.com/amazonswf/latest/developerguide/swf-dg-limits.html
