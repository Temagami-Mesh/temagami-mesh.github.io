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

##  

