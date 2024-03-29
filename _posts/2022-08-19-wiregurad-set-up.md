---
title: Setting up Wireguard for a personal Virtual Private Network.
layout: post
date: 2022-08-19 08:00
---
![The terminal on my mobile phone shown an ssh session to my home network over mobile using wireguard.](/assets/202208/pixelscreen.png)
I just wanted access to my PiHole on my mobile when out of the house so I could get the tracking benefits when out of the house. I was surprised to find Wireguard was relatively simple to set up configure and deploy for my small use case of a desktop, Raspberry Pi, Mobile, iPad, and laptop.
I did run into a small issue with a typo in my config file, which took me an hour to spot, even after I'd spotted it in the script I created to automate adding peers.
<!--more-->
First thing is some basic set up and admin, install wireguard (and the ufw firewall as I am using a Raspberry Pi) and a QR code generation tool I'll use later for adding mobile devices:  
`sudo apt install wireguard wireguard-tools ufw qrencode -y`

First hing is to configure the firewall, so the necessary ports ready to go and the Pi is protected:  
`sudo ufw allow 22/any # ssh`  
`sudo ufw allow 53/any # pihole dns`  
`sudo ufw allow 80/tcp # pihole`      
`sudo ufw allow 4711/tcp # pihole`   
`sudo ufw allow 47777/ # wireguard port`  

I discovered after much troubleshooting that I was using the wrong firewall rules within my configuration file as I was not using IPtables that the rules were deigned to forward traffic between the wireguard network and my specified network. I discover tat I needed to add a `UFW route` rule, which allows the server to now pass traffic to my non Wireguard devices on the home network:

`sudo ufw route allow in on wg0 to 192.168.0.0/24`

Then I enabled the firewall with:  
`sudo ufw enable`  
It should return `Firewall is active and enabled on system startup`.

Next to enable IPv4 forwarding so we can access the local network via wireguard, need to uncomment a couple of lines in `/etc/sysctl.d/99-sysctl.conf`, uncomment:  
`net.ipv4.ip_forward = 1`  
`net.ipv6.conf.all.forwarding = 1`  
Check it's working by running `sudo sysctl -p`, it should return the above lines that were uncommented.  
   
I changed the interface setting son the Pi-Hole to be bound to eth0, so it will listen for DNS from devices connected through Wireguard outside of the home network, it's on the [DNS settings][piholedns] page.  
Now to move onto the fun part and setup Wireguard.

As the wireguard directory is root privilege only, we'll jump into a root shell and set up the server:  
`sudo su -`  
`cd /etc/wireguard`  
`umask 077`  

Next I generated a public private key pair and created the server configuration file.  
Generate the public private key pair:  
`wg genkey | tee server.key | wg pubkey > server.pub`

Create the config file and add some basic config to it:    
`vim /etc/wireguard/wg0.conf`  

Add the following, the IP address can be whatever you want it to be, I've used the 10.x.x.x ranges as my static network is on the 192.x.x.x range, of course IPv6 is optional. The port is also anything you'd like in the higher range that your not already using:  

{% highlight conf %}
[Interface]
Address = 10.10.0.1/24
ListenPort = 47777 \#(note: this can be port number)
\# this is to allow routing to the an internal network from the server (verbatim from [PiHole documentation][pihole])
PostUp = nft add table ip wireguard; nft add chain ip wireguard wireguard_chain {type nat hook postrouting priority srcnat\; policy accept\;}; nft add rule ip wireguard wireguard_chain oifname "eth0" counter packets 0 bytes 0 masquerade; nft add table ip6 wireguard; nft add chain ip6 wireguard wireguard_chain {type nat hook postrouting priority srcnat\; policy accept\;}; nft add rule ip6 wireguard wireguard_chain oifname "eth0" counter packets 0 bytes 0 masquerade
PostDown = nft delete table ip wireguard; nft delete table ip6 wireguard
{% endhighlight %}

Then append the previously generated private key to the config file:  
`echo "PrivateKey = $(cat server.key)" >> /etc/wireguard/wg0.conf`

And exit from your sudo session, that's the basic server setup. Now it's time to register and start the server. Run these three commands to register the server as `wg0` and start it:  
`sudo systemctl enable wg-quick@wg0.service`  
`sudo systemctl daemon-reload`  
`sudo systemctl start wg-quick@wg0`  

Now run `sudo wg`to see if the server is running, you should have returned:  
{% highlight bash %}
interface: wg0
  public key: YOUR_PUBLIC_KEY
  private key: (hidden)
  listening port: 47777
{% endhighlight %}

The following commands will now enable and disable Wireguard, I found it is required to bring the interface down and up after adding a client:  
`sudo wg-quick up wg0`  
`sudo wg-quick down wg0`  

Next is to set up your peers, to do this I created as script as it's a lot of easy to mess up commands and editing config files, so automating it seemed like the approach least likely to fail. It also generates QR code to easily add mobile devices. You can find my script on Github [here][script].

I'm using the official app on my Android mobile, it's basic but dose what is needed download [here][android].  
For iOS I am also using the official app which you can download [here][ios].  

I installed the apps and scanned the QR code to add the configuration to them. Enable the interface and I had issues, but I found it was all to do with typo's in my script. Once they were eradicated I had a working virtual network, that keeps me connected wherever I am.  
To test your connections is working `sudo wg show`will show all connected peers, a peer looks like this:  

{% highlight bash %}
peer: PEER_PUBLIC_KEY
  preshared key: (hidden)
  endpoint: PUBLIC_IP:RANDOM_PORT
  allowed ips: CLIENT_IP
  latest handshake: 3 minutes, 46 seconds ago
  transfer: 46.53 KiB received, 69.89 KiB sent
{% endhighlight %}

This was a great learning experience, these instructions were pulled from various sources including Pi-Hole [documentation][pihole], reddit and stack overflow. I knew almost nothing about Wireguard before this, but now understand how it works and the general bugs that crop up.

[script]: https://github.com/aircooledcafe/wireguard-peer-script/
[pihole]: https://docs.pi-hole.net/guides/vpn/wireguard/internal/
[android]: https://play.google.com/store/apps/details?id=com.wireguard.android
[ios]: https://apps.apple.com/us/app/wireguard/id1441195209
[piholedns]: http://pi.hole/admin/settings.php?tab=dns
