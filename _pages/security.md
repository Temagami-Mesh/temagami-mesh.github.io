---
title: "Security, Encryption and Privacy"
permalink: /security/
excerpt: "Understanding Meshtastic Security, Privacy and Encryption"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

## Public/Private Key Cryptography

Meshtastic direct and admin messages are sent using public key cryptography. Each user is assigned a randomly generated public and private key, which form a key pair. The public key can and should be publicly shared. The private key should be kept private. 

Your identity on the mesh is determined by your public key, not your node name or node id. To exchange public keys with a radio, click on the "Exchange Node Info" button on your Meshtastic App.

Your public and private key pair can be moved to a different radio. You should back these keys up in a secure location in case your radio becomes damaged or destroyed and you need to restore these keys to a different device.

## Encryption

The Meshtastic organization provides this detailed overview of [Meshtastic Encryption](https://meshtastic.org/docs/overview/encryption/)

Meshtastic does not provide Pefect Forward Secrecy at the moment. This means if your public/private key are compromised, any previous messages you sent can be decrypted. {: .notice--warning}

Meshtastic does not provide user authentication. It is difficult but not impossible for a node to impersonate other nodes. {: .notice--warning}

## What Information is Unencrypted?

Message payloads are encrypted, but routing information is not. 

A hypothetical attacker can determine the following information from unencrypted metadata:
- Sender node ID, and the identity of the sender if it is known who is associated with that node id
- The recipient node ID (in the case of direct messages), or the channel on which the message is being sent
- What type of service is being used (MAP, TEXT, TELEMETRY, POSITION, etc)
- The exact length of any text messages sent
- What time the message was sent
- The hop count and hop limit of the message, which can reveal information about the distance the message travelled
- The RSSI and SNR of signals received

## Location Sharing Privacy

Position, Telemetry, and Map Info data published to the regional mesh will be forwarded to publicly accessible map services when they meet the following criteria.

1. They originate from within the region, as determined by a geofence filter algorithm. (Approximately 150km from the center of Lake Temagami)
2. The precision of the location is >300m. Exact locations will not be forwarded to public maps for privacy reasons.
3. The packet is not encrypted, is encrypted by a publicly available key (LongFast, Weather), or the Channel owner has chosen to provide their private key to the mesh administrators.
4. The owner of the radio has configured their radio to allow position sharing over the channel.

To turn on/off your location sharing to public map services, go to the Channel settings for LongFast (or other relevant channel) and enable or disable location sharing on that channel.

It is possible to share your location only with members of your own private channel but it will not show up on any web based maps.
