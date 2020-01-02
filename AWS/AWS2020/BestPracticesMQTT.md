title: AWS - Design MQTT Topics for AWS IoT Core
date: 2020-01-02 11:37:23
tags:
- AWS
- IoT
- MQTT
---

# References

https://d1.awsstatic.com/whitepapers/Designing_MQTT_Topics_for_AWS_IoT_Core.pdf



# MQTT Communication Patterns

* Point to Point
  * different devices subscribe to the topic relevant to itself
* Broadcast
  * multiple devices subscribe to same topic
* Fan-in
  * multiple devices publish to same topic
  * avoid using fan-in to a single end device (?); use fan-in to route a large fleet of messages via IoT Rules Engine.
    * because this routing may hit a non-adjustable limit on a single device MQTT connection (!!!)


# MQTT Communication Patterns

* device to device
* device to cloud
* cloud to device
  * include session information for tracking purpose
* device to/from users


# MQTT Design Best Practices

## General Best Practices

* topic level: lowercase letters, numbers and dashes
* general to specific
* include any relevant routing information in topic
* __prefix__ to distinguish data and command topics
* document topic structure as operation practices
* use IoT Thing name as MQTT client ID -- easy to correlate for logging and policy purpose
* including Thing Name in any MQTT message published by a thing or sending to a specific thing
* review the limitations
  * https://docs.aws.amazon.com/general/latest/gr/iot-core.html
* include contextual information in payload messages
* avoid fan-in to a single device -- do not allow a single device subscribe to a shared topic (!!!)
* never allow device to subscribe to all topics (#); Use single level wildcard (+) for IoT Rules

## Best Practices for Telemetry

* IoT Basic Ingest for Telemetry
   * topic is designed to help route to different rules in rule engine (no need for device-2-device)
* Traditional MQTT topics

```
dt/<application>/<context>/<thing-name>/<dt-type>
```

<application>: useful for version switch
<context>: grouping ; for example device group id
<dt-type>: subcomponent of device / sensors


## Best Practices for Commands

* IoT Shadow

* AWS IoT Shadow is the preferred AWS IoT Service for implementing individual device
commands.
* AWS IoT Device Jobs(?) should be used for fleet-wide operations as it
provides extra benefits, such as Amazon CloudWatch metrics for Job tracking, and the
ability to track multiple in-transit Jobs for a single device.
* You can use a combination of
the AWS IoT Shadow, AWS IoT Job documents(?), and standard MQTT topics to support
your command use cases.

## Best Practices for Using the AWS IoT Shadow

* Don't share shadow
* Shadow is for infrequent state or command happen in min/hour/day.
* Use shadow for storing status metrics of device
* Use shadow for firmware version (major.minor.patch)
* use clientToken field for tracking purpose

## Best Practices for using IoT Jobs for commands

IoT Job contains instructions that the thing must run to complete it's tranction.

* Use thing group with AWS IoT Jobs
   * update all things with certain firmware
* Use staged rollout using Device Jobs

## Best Practices for using MQTT Topics for commands

```
cmd/<application>/<context>/<destination-id>/<req-type>
cmd/<application>/<context>/<destination-id>/<res-type>
```

* Command Payload Syntax
   * session id
   * response-topic

# Applications on AWS
