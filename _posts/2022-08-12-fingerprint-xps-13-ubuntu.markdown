---
layout: post
title: The fingerprint reader conundrum.
date: 2022-08-12 08:00      
tags: xps 13 fingerprint reader goodix ubuntu lts 22.04
---

I had a really annoying problem where I couldn't get the fingerprint reader working on my Dell XPS 13 after installing Ubuntu 22.04. Turns out there was a bug in the released drivers and Dell hadn't picked it up when certifying the release for this device. My fingerprint reader is a Goodix Technology one, which doesn't offer open source drivers itself either so couldn't fall back to those.  
  
So I eventually found this bug report over on [launchpad][bugthread], where it turns out a fix has been found and released to the staging branch of ubuntu (it was supposed to be included in todays 22.04. release though it didn't work out the box when I updated).  
  
I had to do a few extra steps to get it working, fist I added the PPA for the pre-release version of the driver (the repository is [here][staging]):  
  
`sudo add-apt-repository ppa:andch/staging-fprint`  
  
Then I updated and installed the correct version for my fingerprint reader:  
  
`sudo apt update && sudo apt install libfprint-2-tod1-goodix libfprint-2-tod1`  
  
A quick log out and log back in and the fingerprint reader was working like a dream. I then just had to enable it for `sudo` which was as simple as running PAM (Pluggable Authentication Module) to update it's config:  
  
`sudo pam-auth-update`  
  
Use `space` to toggle and enable 'Fingerprint authentication' and your good to go with using your fingerprint to authorise `sudo` commands.  


[bugthread]: https://bugs.launchpad.net/ubuntu/+source/libfprint/+bug/1966911
[staging]: https://launchpad.net/~andch/+archive/ubuntu/staging-fprint?field.series_filter=jammy