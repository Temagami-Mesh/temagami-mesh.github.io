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

## Project Maintainers:

Technical assistance for this mesh networking project is being provided on a voluntary basis by:

Bond Keevil (support@bondkeevil.ca) 
- Amateur Radio Operator Callsigns VA3HBK and VE3TLV
- CompTIA Network+ Certified
- Public Key: LJpD2FFLkKokaV28E7V2IP269sP3Ni3S1xd2msXc+lE=

If you add a person's public key to you list of authorized admins in the Meshtastic app settings, they can help you administer your radio remotely over the Mesh network. 

## How to Support the Network

* Install a meshtastic node on your property. Ideal locations are elevated and have a reliable year round power source. 
* Monetary donations to be used to build and distribute free nodes. The parts to construct a reliable commercial quality node cost approximately $300. Interac E-Transfer to me@bondkeevil.ca
* Donations of meshtastic node sites. Ideal node sites are elevated and have a reliable power source. Nodes can also be installed in vehicles and vessels. Solar nodes can be built but its a challenge to keep them operating in sub-zero temperatures.
* Donations of organizational support. If aspects of this mission align with your organizational goals, please go ahead and implement your own projects.

Please note that no formal organization exists to control this project or own property. All contributions remain the property and responsbility of the contributors. 
