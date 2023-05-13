---
layout: post
title: Wireguard Peer Config and Setup
date: 2023-05-13 08:00
tags: android, wireguard, ios, iphone, security, privacy, vpn
---

## Wireguard Peer Config and Setup

This is some basic instructions for setting up Peers for Wireguard when routing traffic through a central interface. In my circumstances it is mostly for accessing my home network and using my DNS servers when on mobile. I am working on a setup with Wireguard installed on my OPNSense router (setup out of scope).

Install wireguard:  
`sudo apt install`

Set up the link:  
`sudo ip link add dev wg0 type wireguard`

Generate a public/private key pare:  
`wg genkey | tee private.key | wg pubkey public.key`

Add the information to the config file in the wireguard directory:  
`/etc/wireguard/wg0.conf`  
Replace `wg0.conf` with the name of the connection if not `wg0`.

Example `.conf` file:

```bash
[interface]
# Host Private Key
PrivateKey = alkdfnaXdsaoDcasdkcnmaldop0o3240fd2030293j2=
# Host IP address
Address = 10.10.10.1/32
# Global DNS server to use
DNS = 1.1.1.1

# List any peers here
[Peer]
# Serve/Peer public key
PublicKey = er8M9cwuahdnfalkdfhaoisdnfalGhRETPqpvCjiCljis=
# The IPs to route over wireguard 0.0.0.0/0 for all, or other CIDR ranges
AllowedIPs = 0.0.0.0/0
# The public endpoint of the wireguard server
Endpoint = 42.47.11.38:12345
```

Pass through qrencode to scan into the mobile app if necessary:  
`sudo apt install qrencode`
`qrencode -t ansiutf8 < wg0.conf`

It is worth moving the `private.key` to the wireguard directory and removing permissions from it and the `.conf` file so only `root` user has any access to them, ensure private keys are kept secret:  
`sudo chmod go= /etc/wireguard/wg0.conf`
`sudo mv private.key /etc/wireguard/private.key && sudo chmod go= /etc/wireguard/private.key`

Test the connection, enable wireguard connection:  
`sudo wg-quick up wg0`

Disable wireguard connection:  
`sudo wg-quick down wg0`

### Some useful links

Wireguard GUI for debian based linux distributions, adds system tray icon and simple toggle:  
https://github.com/UnnoTed/wireguird

OPNSense Wireguard Road Warrior setup, I used most steps to set up on my router:  
https://docs.opnsense.org/manual/how-tos/wireguard-client.html

Wireguard homepage:  
https://wireguard.com

Digital Oceans Wireguard setup guide is very useful and more featured than Wireguards' own one:  
https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04
