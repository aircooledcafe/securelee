---
layout: post
title: Generating and Adding SSH Keys
date: 2022-08-10 11:40
tags: github ssh keys security ed25519 linux
---

I always forget to transfer my keys and never remeber the command s to generte new ones, So here are my basic instructions for generating SSH keys and setting them up with Github and a server.
<!--more-->  
Generate your keys (`-C "comment"` optional):  
`ssh-keygen -t ed25519 -C "my ssh keys for github"`  
  
Follow onscreen instructions, providing a passphrase and location to store the public/private key pair.  
    
Ensure the SSH agent is started:    
`eval "$(ssh-agent -s)`  
Should return the pid of the agent e.g.:  
`>Agent pid 12345`  
 
Add your keys (replace `id_ed25519` with file name of your ssh keys):  
`ssh-add ~/.ssh/id_ed25519`  
  
Now add the public key to github. Go to:  
  
Settings > SSH and GPG Lyes > New SSH Key  
  
Paste in your SSH key, your good to go.
  
To add your public keys to a server I will be using my Raspberry Pi:  
  
`ssh-copy-id -i ~/path/to/ssh/key USER@IP_ADDRESS`  
  
Now you can log in to the remote device using your SSH keys with:  
  
`ssh IP_ADDRESS`

Command to remove a host from your "known_hosts" file:
`ssh-keygen -f "/home/ben/.ssh/known_hosts" -R "10.10.10.10"`



