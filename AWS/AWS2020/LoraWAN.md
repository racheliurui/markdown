title: LoraWAN
date: 2020-07-18 11:37:23
tags:
- IoT
- LoraWAN
---

# References

https://youtu.be/8Oxcp9wQQnk

# Terminology

## Lora vs LoraWAN
  * Lora is the protocol, __Lo__ng __Ra__nge ; LoRa is Layer2
  * LoraWAN is the IoT solution based on Lora technology

## Lora Pro/Cons
  * ISM Open frequency(415,868,915MHz, free ; no license required
  * Interference ; low data rate

## Limitations / Parameters

Target: transmission message about 10 km and the battery last for 2 years.

* Frequency: Pay attention to band requirement per country  
* Tx power (transmission power): 2-14 dbm / 5-20 dBm; the higher the power , the longer distance signals can cover
* Bandwidth (125/250/500 KHz): the higher the more data can be include in one transmission; the higher the bandwidth, the shorter battery life, the shorter range and more interference.(??); checked the local laws
* spreading factor: (7-12), the larger spreading factor, the longer distance and shorter battery life.
* coding rate: 4/5, 4/6, 4/7, 4/8,
4/5 means 5 error bits used to correct 4 bit of data. The more coding rate, means your data can transfer longer distance but lower battery life.


## LoRa Device

* Lora Nodes:
Normally will integrate sensor, transponder, mircrocontroler all together.
Receive and transmit sensor data, send out via air using LoRa protocol
  * LoPy ; LORA GPS Hat; RN2483


* Gateway :
Receive LoRa data via multi channels with different frequencies ; send out data to IP network.
  * IMST IC880A-SPI (8 channels at a time)


## LoRaWAN

Layer 3 and 4 ;  
