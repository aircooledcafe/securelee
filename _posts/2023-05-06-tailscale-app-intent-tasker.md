---
layout: post
title: Tailscale App Intent Settings For Automation
date: 2023-05-06 09:00
tags: android tailscale automation vpn 
---

Settings for Android App intents for conecting and disconnecting Tailscale VPN, for use in automation like Tasker.
I use it to connect to Tailscale when I leave my home WiFi network and disconnect again when join the home network.
These settings can be used to in the "Send Intent" action within Tasker. Any fields not mentioned should be left blank.

Tailscale Up

  Action: com.tailscale.ipn.CONNECT_VPN
  Package: com.tailscale.ipn
  Class: com.tailscale.ipn.IPNReceiver
  Target: Broadcast Receiver

Tailscale Down:

  Action: com.tailscale.ipn.DISCONNECT_VPN
  Package: com.tailscale.ipn
  Class: com.tailscale.ipn.IPNReceiver
  Target: Broadcast Receiver

These chould be set up as tasks within Tasker, which can the nbe called in the Profiles you create.
I created a connected and disconnected profile for my home network which called the appropriate intent.
