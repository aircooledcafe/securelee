---
layout: post
title: iOS Custom URL for VPN and DNS Shortcuts
date: 2022-07-28 08:00
tags: ios, shortcuts, url, vpn, dns
---

I was trying to get an easy way to automate toggling my Wireguard VPN conection on and off when I left the house or for certain times of day. Unfortunately wiht the standard iOS settings or the Wireguard app there is no way to autmoate this that I have found. I did however discover some custom URLs for getting to certain VPN and DNS settings via shortcuts. So I can get all the way to the VPN setting but there is no way to make it toggle as far as I can tell. here are all the URL's I discovered while looking into this, as most guides still refer to the VPN shortcut the old way. I found [this][shortcuts] list useful in my research, though it is out of date.
<!--more--> 
To jump to the VPN settings:
`prefs:root=General&path=ManagedConfigurationList/VPN`

TO jump to a specific VPN configuration:
`prefs:root=General&path=ManagedConfigurationList/VPN/Name%20of%20vpn`

For example I have a wireguard configuration called "iPhone", so to jump to that the URL would be:
`prefs:root=General&path=ManagedConfigurationList/VPN/iPhone`

You can also add or delete VPN configurations from here too.
Add configuration:
`prefs:root=General&path=ManagedConfigurationList/VPN/Add%20VPN%20Configuration%E2%80%A6`

Delete a configuration:
`prefs:root=General&path=ManagedConfigurationList/VPN/Name%20of%20vpn/Delete%20VPN`

To jump to the DNS settings, there is no way I have found to do naything more specific with DNS settings yet:

`prefs:root=General&path=ManagedConfigurationList/DNS`

[shortcuts]: https://www.reddit.com/r/shortcuts/comments/i9rjbh/an_updated_list_of_settings_urls/