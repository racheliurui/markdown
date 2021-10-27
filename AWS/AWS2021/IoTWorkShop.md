title: AWS - IoT Edukit
date: 2020-01-18 11:37:23
tags:
- AWS
- IoT
- workshop
---


## IDE Env Setup

1) Device Driver
2) PlatformIO plugin for VSCode
3) ESP Rainmaker App

## Lab Summary 

### Lab 1: 
Install ESP RainMaker Agent on device to be able to directly talking with ESP Rainmaker SAAS and 

https://rainmaker.espressif.com/docs/claiming.html#assisted-claiming-esp32

* Assisted claiming process (???)


### Lab 2: Cloud Connected Blinky

* Using registration_helper.py to register the device to aws IoT Core
* Pub/Sub to the devices
   *  The device will continously send random reading to IoT Core
   *  The device can blink the LED when received message from cloud

### Lab 3: Samrt Thermostat (IoT Core + IoT Events)


### Lab 4: Smart Spaces (IoT Core + IoT Analytics + Sagemaker Studio autopilot)

## Reference Links

https://edukit.workshop.aws/
https://rainmaker.espressif.com/