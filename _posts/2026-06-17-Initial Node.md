---
layout: single
title: "First Permanent Infrastructure Node"
date: 2026-06-17
categories: posts
---

This is the infrastructure node that I am hosting on my own property. 

## Design Considerations ##

- A permanent infrastructure node that should run 24 hours a day, 365 days per year.
- There is a reasonable power source nearby. 
- A bi-directional RF amplifier to experiment with the effects of boosting the signal
- Antenna height can be a maximum of 15' above water level, which is fairly low.

## Range Tests ##

![Image of Component Assembly](http://temagami-mesh.github.io/assets/images/first_node_range.jpg)

The range of the device was reliable to about 6.5 kms. From 6.5 to 12km the range was somewhat spotty. There is a radio shadow behind many of the larger islands, suggesting that nodes should be put on the shore that faces the other nodes that you are trying to reach.

Doubling the output power of the transmitter increased the range by about 1km, which was far less than I expected.

These range test readings are for a consumer handheld device held by a person. A link between two well designed infrastructure nodes can probably expect to have a range of 15-20kms on the lake. I was able to get a strong signal at the access road landing using a larger infrastructure node. 

## Components Used ##

![Image of Component Assembly](http://temagami-mesh.github.io/assets/images/first_node_hardware.jpg)

- [WisMesh 1W Booster Starter Kit High-power Meshtastic solution with nRF52840, SX1262, and SKY66122 PA for extended mesh range](https://www.aliexpress.com/item/1005010651127313.html?spm=a2g0o.order_list.order_list_main.10.3a351802KaH4IX)
- [868Mhz 915Mhz 2.4Ghz 5.8Ghz Bidirectional Amplifier 2W RF Signal Booster FOR LORA Helium Miner UAV WiFi Drone Image Transmission](https://www.aliexpress.com/item/1005009625219917.html?spm=a2g0o.order_list.order_list_main.5.75fc1802PJgxG5)
- [DC 12V/24V to 5V USB C Converter - DC-DC Step Down Converter Buck Module](https://www.amazon.ca/dp/B0D8PQZY4J)
