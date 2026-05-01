---
permalink: /about/
title: "About"
---

 This website provides a central repository of information for meshtastic users on Lake Temagami and the surrounding region and a github discussion forum where people can share updates or ask questions.

## What is Meshtastic?

Meshtastic is an open-source, off-grid communication system that uses low-power LoRa radios to form a self-healing mesh network. Small battery-powered nodes relay text messages, GPS positions, and sensor data from device to device without requiring cellular service, internet, or infrastructure. Messages automatically hop across multiple nodes, extending range far beyond a single radio link.

## How this helps the Temagami community:

On a lake area with poor or no cell coverage, Meshtastic nodes placed in homes, cabins, boats, and high points (trees, masts, hills) create a shared community network:

- Reliable text messaging between residents, even dozens of kilometers apart
- Emergency coordination during storms, fires, medical events, or power outages
- Boat-to-shore and shore-to-shore communication across open water (excellent LoRa propagation)
- Communication between group members and for remote worker safety
- GPS location sharing for safety on the water, trails, canoe routes and snowmobile trails
- Environmental sensors (weather, water level, temperature) reporting over the same network
- No ongoing cost and extremely low power draw (solar or battery friendly)

Because each node acts as a relay, the network becomes stronger as more people participate. A few well-placed “router” nodes on high ground or tall trees can cover the entire lake, providing a resilient, community-owned communications layer that continues working when conventional networks fail.

## What You Need to Connect to the Mesh

You need a personal meshtastic radio, called a "companion radio", and a cellular phone with the meshtastic app installed. Your cellular phone connects to the companion radio using Bluetooth and the companion radio relays messages to the meshtastic network. There are a multitude of options available in the $50 to $200 CAD price range on websites like Amazon. 

**Tips when choosing:**

* Always pick the correct **frequency band** for your region (915 MHz for North America).
* Boards with **built-in GPS** help with location sharing in a mesh but should have a switch to turn off GPS tracking.
* Some devices include displays or buttons for easier standalone use; others are “headless” and managed via your phone app.
* For the best range, choose a device with a quality external antenna that can be upgraded.
* Since lithium batteries should not be charged in sub-zero temperatures, nodes for winter use should be connected to an external power supply

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
