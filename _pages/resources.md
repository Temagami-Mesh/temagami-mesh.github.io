---
permalink: /resources/
title: "Resources"
---

## Configuring your Radio

Documentation on how to configure your radio for different use cases are widely available online. To connect to the Lake Temagami Mesh, the following settings MUST be used:

* Region: US (This sets the frequency to 915 Mhz)
* Modem Preset: "LongFast"

All other settings can be left at their default value or modified at your discretion.

IMPORTANT: Do not turn on your radio without an antenna connected. Transmitting from a device with no antenna installed can damage the device.

## How to Extend the Range of Your Mesh

A common configuration is to have a solar or battery powered "base station" node located in a high location somewhere on your property (on the roof of a building, in a large tree, on a pole on a hill). Your personal companion radio would then relay packets through your own base station to other nodes on the network.

The key to long range with LoRa radios is to place the antenna as high as possible, in a vertical orientation, and away from metal objects like roof flashing. 

You should ensure that your antenna is designed for the 915Mhz frequency band and is a high gain antenna design. The short stubby antennas that come with most radios are compromise antennas and should be replaced with a quality antenna if you need to improve your range.

## Vehicle Tracking

It is common to install GPS enabled meshtastic nodes in vehicles of various sorts for use as asset trackers. Most commercially available radios take a USB-C cable as a power source and have an on/off switch to disable the GPS tracking features. Once installed, the radio can be configured to forward the position reports to the Internet where others can view the location though websites like https://meshmap.net

## Environmental Sensor Data

It is possible to buy or build meshtastic nodes that can broadcast data from various sensors to the network:

- Temperature/Humidity Sensors
- Lake Level/Temperature Sensors
- Barometric Pressure Sensors
- Nearby lightning strikes
- Wind speed/direction
- Environment Canada weather alerts
- Any sort of switch or logic state
- Motion or human presence sensors

A channel has been allocated for weather station and sensor broadcasts

## Channels

The technology provides for up to 8 channels or chatrooms that can be encrypted for privacy. You can create your own private channels for your camp, cottage, family, or business. Only people who have the pre-shared key for your channel will be able to access it. Other nodes on the mesh will then help forward your encrypted packets to their intended recipients provided they share the same radio settings (Longfast preset and US/Canada frequencies). 

The two predefined public channels for the area are:  

| No. | Name      | Pre-shared key | Purpose |
| :-: | --------- | -------------- | ------- |
| 1   | Longfast  | AQ==           | Default public channel |
| 2   | Weather   | Rg==           | Automated Weather Broadcasts |
