---
title: "MQTT Settings"
permalink: /mqtt/
excerpt: "Settings to configure bridging between nodes over the Internet using the MQTT protocol"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

## MQTT Bridging ##

The MQTT Internet protocol is used by Meshtastic devices to enable messages and other data packets to be bridged between different meshes that are not within RF range of each other. There are two anticipated use cases for this feature:

  1. You are outside of the Temagami region with your companion radio and want to communicate with meshtastic users in the Temagami region. In this case your device can use your cellular data connection to connect to the Temagami area meshtastic network from anywhere in the world.
  2. Separate meshes exist in the Temagami region but the mesh is too sparse for direct RF communication between meshes. In this case, MQTT can be used to bridge the disparate RF networks to make them appear connected.

##  Global MQTT Configuration ##

Under "Radio Configuration" go to the "LoRa" tab and ensure "OK To MQTT" is turned on and "Ignore MQTT" is turned off. These settings explicitly allow or disallow MQTT bridging globally for all channels.

![MQTT Radio Settings](/assets/mqtt-radio-settings-lora.png)

## Per Channel MQTT Configuration ##

It is possible to enable or disable MQTT bridging for specific channels. Under "Radio Configuration" go to the "Channels" tab and turn on "Uplink enabled" and "Downlink enabled" for any channels you have configured that you want to bridge. You can also choose whether to enable position reporting on each particular channel and how accurately to report the position. In the example below, we are enabling MQTT bridging on the default public channel which is called "LongFast".

![MQTT Radio Settings](/assets/mqtt-radio-settings-channels.png)

## MQTT Module Configuration ##

You need to enable the MQTT module by going to the "Module Config" page and selecting the "MQTT" tab. Ensure that "Enabled" is turned on. All other settings are left at their default values.

We use the default username of "meshdev" and password of "large4cats" to log in to the default mqtt server provided by the Meshtastic community.

Turning on "JSON enabled" will make it easier in the future to develop apps like maps and dashboards using data from the mesh. But it can also be left off, which is the default value.

![MQTT Module Settings](/assets/mqtt-module-settings-enabled.png)

It is important to set the "Root Topic" to "msh/CA/ON/Temagami" to connect to the Temagami area mesh. Other options that are suggested by other online setup guides include "msh/US" in the US and "msh/CA" or "msh/CA/ON" in Canada. Since the purpose of this project is to create a regional mesh for the Lake Temagami area, we have opted for the more specific regional root topic of "msh/CA/ON/Temagami".

In an emergency situation where you need help and are unable to connect to someone locally, changing your root topic to "msh/CA" will enable you to broadcast your emergency message Canada wide.

Turning on "MQTT Proxy Enabled" will let your companion radio use the bluetooth connection of your phone to connect to the Internet to send and recieve MQTT packets.

Turning on "Map Reporting Enabled" will forward additional information about your node to Internet web services such as https://meshmap.net/

![MQTT Module Settings](/assets/mqtt-module-settings-topic.png)

## Neighbour Info Module ##

You may want to consider enabling the Neighbour Info module, which forwards information about which nodes your node can here to the Internet using MQTT so that a connection graph can be displayed of the local mesh. This is for a future planned feature and AFAIK this connection graph app does not yet exist.
