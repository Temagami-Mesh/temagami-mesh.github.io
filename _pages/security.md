---
title: "Security and Privacy"
permalink: /encryption/
excerpt: "Understanding Meshtastic Encryption"
header:
  overlay_image: /assets/images/temagami_mesh_hero.jpg
  overlay_filter: 0.35
  caption: "Connecting Temagami through resilient mesh networking"
---

The Meshtastic organization provides this detailed overview of (Meshtastic Encryption)[https://meshtastic.org/docs/overview/encryption/]

Meshtastic does not provide Pefect Forward Secrecy at the moment. This means if your public/private key are compromised, any previous messages you sent can be decrypted.

Meshtastic does not provide user authentication. It is difficult but not impossible for a node to impersonate other nodes. 

## Public/Private Key Cryptography

Direct messages and administrative packets are sent using public key cryptography. Each user is assigned a public and private key which are a key pair. The public key can and should be publicly shared. The private key should be kept private. 

Your identity on the mesh is determined by your public/private keypair, not your node name or node id. 

Your public and private key pair can be moved to a different radio. You should back these keys up in a secure location in case your radio becomes damaged or destroyed and you need to restore these keys to a different device.

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

