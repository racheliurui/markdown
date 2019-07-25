title: AWS - Amazon Lambda
date: 2019-07-25 11:37:23
tags:
- AWS
- Lambda
---

# A Serverless Journey: AWS Lambda Under the Hood

## Lambda Load Balancing

![lambda_components](https://github.com/racheliurui/markdown/blob/master/Trending/AWS2019/images/lambda_components.png?raw=true)

* __Front End Invoke__: authentication the caller, load configs & env ; confirm concurrency with __Counting Service__
* __Counting Service__: Region wide view of concurrency to help set limits (quorum protocol, 2/3 agreement protocol ); <1.5 milliseconds response time
* __Worker Manager__ : assume role, track the container lifecyle (running, idle) and maintain the worker pool
* __Worker__ : provision sandbox and download customer code and run;
        *  warm sandbox means the sandbox finished previous run
        *  sandbox is equivalent of docker image
* __Placement Service__: provision worker


* Example,
   *  Fannie Mae scale to between 20 and 50,000 concurrent executions over minutes.

## Lambda Handling Failures

* Multi-AZ

## Security Isolation

![lambda_layers](https://github.com/racheliurui/markdown/blob/master/Trending/AWS2019/images/lambda_layers.png?raw=true)


* EC2 as worker level
* EC2 Bare Metal as worker level (no hardware share with other account)
  * Firecraker mode

## Managing Utilization

* Keep the server busy
* Utilization is handled by AWS
   * Lambda have different algorithm to spread the load (concentrate the load)
   * Lambda Pack different workload into one server to avoid similar workload spike all together.
   * TTTTTTTTTTTTTTTTTTO

## Lambda benefit


* Load Balancing
* Auto Scaling
* Handling Failures
* Security Isolation
* Managing Utilization


# Reference

>
https://youtu.be/QdzV04T_kec
