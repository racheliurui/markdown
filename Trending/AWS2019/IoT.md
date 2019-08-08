title: AWS - IoT
date: 2019-07-23 11:37:23
tags:
- AWS
- IoT
- WPA3
---


# Reference

> https://youtu.be/LbeWdLaXYDo

# Home automation ; Home security ; Home networking

FreeRTOS / Greegrass --> IoT Core, management, Analytics / Database, ML --> IoT applications

## DEMO from Vestel

* VESTEL
* Dedicated IoT group
* Highlight of current archi
   * Use IoT Core
   * Use API Gateway to support service for both Alexa and GoogleHome
   * Use lambda to run logic against IoT Core and try serveless

## Simplify large number of IoT devices

* WPA3 Specification, new device provision protocol
* By using the mobile to scan the barcode to get the public key of the device ; then  the router automatically allow the device to connect to internet.

## Home Security & Monitoring

* Amazon FreeRTOS,
* AWS Greengrass, allows local RTOS communicate each other
* SageMaker: training the model --> export model to S3
* IoT Core, create a rule,  subscribe sound from rule and assign to lambda to call the trained model to detect the sound.
* Push the model to greengrass (local) , then device can push the data to local greengrass to run the same the lambda function.
* Greengrass discovery -- a green grass device can discover and connect with the greengrass device


## Home networking

* Greengrass as a hub
* Use IoT , using Device Defender , to detect unusual publishing

# Reference

> https://youtu.be/HEQkVHxu46A

## Overview

### Edge / Endpoint : Amazon FreeRTOS

* OTA: over the air

### Device Gateway: GreenGrass Core

* Can be on-promise or in cloud
* Protocol : MQTT, WebSockets, HTTP
* TLS 1.2 Only
* Message Broker

### Device Management: IoT Device Management

* Batch Fleet Provision
* Search device

### IoT Device Defender

* Audit Device Config /monitor / Identify Anomalies / Alerts/ Patch
* For example, security best practice check (certificate sharing )

### IoT Analytics

* Pipelines --> Analysis / ML

### Other features

* 1-Click , provisioned device . like the aws purchase button.

## Demo -- Modjoul

* 8 sensors , 50 MB data per person per day
* 2 weeks data storage locally
* Use IoT Analytics replace EMR
