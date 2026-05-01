---
permalink: /connect/
title: "Connect"
---

You need a personal meshtastic radio, called a "companion radio", and a cellular phone with the meshtastic app installed. Your cellular phone connects to the companion radio using Bluetooth and the companion radio relays messages to the meshtastic network. There are a multitude of options available in the $50 to $200 CAD price range on websites like Amazon. Popular vendors include [Elecrow](https://www.elecrow.com/meshtastic.html), [RAK Wireless](https://store.rakwireless.com/collections/meshtastic), [Rockland](https://store.rokland.com/pages/meshtastic-hardware-rak-lilygo?srsltid=AfmBOorPQbvCZxGJdaE7NkHXq8WXAthKINd0iNje8IXi9QIGBaBSHeWJ), and [Heltec](https://heltec.org/product-category/lora/meshtastic/).

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

If you own property or vehicles on or near Lake Temagami, you could consider hosting one or more specialized meshtastic nodes: 

* Router nodes are placed in high locations and are designed to be reliable and permanent parts of the meshtastic network infrastructure
* Sensor nodes broadcast data from weather stations or any off-grid devices that need to be monitored
* Tracker nodes contain GPS chips and can be installed in cars, boats, snowmobiles or carried on your person

Please contact one of the project maintainers if you need help with this. A limited number of free nodes will be distributed to help establish this network.

No formal organization exists to control this project or own any property. All contributions remain the property of and are the responsbility of the contributors to maintain and develop.

If any aspect of this mission aligns with your organizational goals, please go ahead and implement your own projects using the network. You don't need permission. Some example uses of how a business organization might use the technology:

* Tracking the GPS location of your vehicles on https://meshmap.net/
* Providing for the safety of remote workers or their customers
* Monitoring water levels, temperatures, and equipment

IMPORTANT: Do not turn on your radio without an antenna connected. Transmitting from a device with no antenna installed can damage the device.
