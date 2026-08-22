---
title: "MQTT Setup Guide"
permalink: /mqtt/
excerpt: "Configure your radio for Internet bridging to the mesh"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

# MQTT Setup Guide

The Message Queuing and Telemetry Transport (MQTT) protocol allows users to publish messages to the Internet and for other users to subscribe to them. Meshtastic uses MQTT to:

1. Route meshtastic messages between disparate RF mesh networks that cannot be connected via other means
2. To allow meshtastic users who don't have direct access to a regional RF mesh network to use the service via an Internet or cellular connection
3. To publish location data to publicly accessible Internet mesh map services (for those who have configured their radios to do so).

The Temagami Meshtastic Project operates its own private MQTT data broker, which needs to be programmed into your radio to connect to the local mesh. There are other public data brokers available, including one that is provided by default by the Meshtastic organization. Unfortunately the public broker has highly restrictive policies that made it unuseable for our situation. 

## Requesting an MQTT Account

Our convention is to set up an account for each node where the username is the node's nodeid in hexidecimal format. This allows us to disable misbehaving or compromised nodes and avoids the kind of restrictive security policies that have made the public broker unuseable.

Your node ID can be obtained from many places such as the User Config screen of the cellphone app. This is typically a number that looks like: !50b5a1dd. Your username will be this node id (without the exclamation mark).

Once you have obtained your node id, send an email to temagami-mesh.tutamail.com to request an mqtt broker account for your node. Your account will be set up with a randomly generated password which will be emailed to you. It is probably a good idea to save that password as we don't keep any records of assigned MQTT passwords.

## Required Private MQTT Server Settings

**URL:** mqtt.temagami-mesh.net

**Username:** 50b5a1dd (Substitute your own node id)

**Password:** Kf_1a598Rb&n (Substitute your assigned random password)

**TLS Enabled:** False

**Root Topic:** msh/temagami

## Optional MQTT Server Settings

**Map Reporting:** If turned on, your radio will send periodic updates to Internet map servers that includes additional information about your node. This data is sent unencrypted as it is intended to be displayed publicly.

**Position Accuracy:** Set the degree of precision your position will be reported to public map services as. The most accurate is 729mm, which provides a degree of privacy protection while still advertising the existence of nodes in your area.

**JSON Output Enabled:** Only for nodes that contain sensor or weather data intended to be processed by our systems (or your own systems). Turn this on if your node will be participating in an IOT project, off otherwise.

**Encryption Enabled:** Whether data sent to the server will be encrypted. If your data is encrypted, the message content cannot be accessed or processed by the server but it will be forwarded to other nodes who might have a decryption key for it. 

## Public and Regional Map Reporting Services

Our system maintains a regional list of node position reports. There is a geofence filter applied to this to exclude nodes that are more than 150-200km from the centre of Lake Temagami. This allows Temagami mesh users to see other node positions within the region without cluttering up their map with nodes from distant lands.

We also automatically forward MAP, POSITION, and TELEMETRY packets from your device to publicly accessible mesh mapping services provided the following criteria are met:

1. The coordinates have to be within our region as determined by the current geofence settings of our system.
2. Your reported position cannot be more accurate than 300m.
3. Your data must be sent to the server unencrypted. (You could also provide the admins with the decryption key for your channel.)
