---
layout: post
title: How to setup your Yubikey for Sudo Authorisations
date: 2023-05-13 08:00
tags: linux, yubikey, yubico, security, sudo, u2f, pam.d, terminal
---

The basics of adding a Yubikey to be used to authorise `sudo` commands on a Debian Linux machine.

First install the required `pam` module:  
`sudo apt install libpam-u2f`

Create the configuration folder for the U2F keys config file:
`mkdir ~/.config/Yubikey`

Register your Yubikey into the configuration file in the directory you created:  
`pamu2fcfg > ~/.config/Yubikey/u2f_keys`  
You will be prompted for your keys pin (if set, it should be!) and required to touch the key as normal to register it.

To register additional keys use the following command:  
`pamu2fcfg -n >> ~/.config/Yubico/u2f_keys`

Now you need to add a line to the `/etc/pam.d/sudo`file to enable the key.  
Add the line `auth sufficient pam_u2f.so` after the `session` entries and before the line `@include common-auth`, then save but **do not close** the file.  
If you want to make it 2FA instead of just authorisation the line would be `auth required pam_u2f.so`.

Open a new terminal window to test the configuration is working:
`sudo echo SUCCESS`

If that works as expected, waiting on the touch of the Yubikey you are now OK to close the `/etc/pam.d/sudo` file, if not check for typos or revert before debugging.

These are instructions simplified for the one purpose I was after from the Yubico guide, which goes into more details and use cases..
[https://support.yubico.com/hc/en-us/articles/360016649099-Ubuntu-Linux-Login-Guide-U2F](https://support.yubico.com/hc/en-us/articles/360016649099-Ubuntu-Linux-Login-Guide-U2F)
