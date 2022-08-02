---
layout: post
title: Setting up ZSH on linux with antigen
date: 2022-08-02 20:00 +0100
tags: linux zsh antigen 
---

Here is my setup for my shell in linux, I use [antigen][antigen] to manage plugins and [Powerlevel10k][powerlevel10k] them by [Roman Perepelitsa][romkatv]. All commands assume you are in your home directory.  

First things first is to install some dependencies:  

`sudo apt install zsh git curl autojump fonts-powerline wget`  

(If your not on Ubuntu or Debian, you can follow instructions [here][powerline] for powerline fonts.)  

Next up is to get antigen:  

`curl -L git.io/antigen > antigen.zsh`  

Then we need to create our zshrc file and add a couple of lines to enable antigen:  

`vim .zshrc`  

And add the following lines:  

`source /home/vm1/antigen.zsh   
antigen init ~/.antigenrc`  

Now we need to create our antigenrc file and configure our plugins:  

`vim .antigenrc`

Add the following lines:

{% highlight conf %}
# Load the oh-my-zsh's library.  
antigen use oh-my-zsh  

# Bundles from the default repo (robbyrussell's oh-my-zsh).   
antigen bundle git  
antigen bundle pip  
antigen bundle lein
antigen bundle command-not-found
antigen bundle docker
# use the j autojump directory function
antigen bundle autojump


# Syntax highlighting bundle.
antigen bundle zsh-users/zsh-syntax-highlighting
antigen bundle zsh-users/zsh-autosuggestions
antigen bundle zsh-users/zsh-completions
# double esc to add sudo
antigen bundle hcgraf/zsh-sudo
# colored man pages
antigen bundle ael-code/zsh-colored-man-pages

# Load the theme.
antigen theme romkatv/powerlevel10k

# Tell Antigen that you're done.
antigen apply
{% endhighlight %}

Configure powerlevel10k:  

`p10k configure`

Change the default shell to zsh:  

`chsh -s /usr/bin/zsh`   

Relaod shell for changes to take effect.

Here are links to MesloLGS, one of my prefered fonts and the default powerline fault used by powerlevel10k, ideal for manual installation if your not on Ubuntu or Debian:  
[MesloLGS Regular][mesloregular]  
[MesloLGS Bold][meslobold]   
[MesloLGS Italic][mesloitalic]  
[MesloLGS Bold Italic][meslobolditalic]  

[mesloregular]: https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf
[meslobold]: https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf
[mesloitalic]: https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf
[meslobolditalic]: https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf
[powerlevel10k]: https://github.com/romkatv/powerlevel10k
[antigen]: https://github.com/zsh-users/antigen
[romkatv]: https://github.com/romkatv
[powerline]: https://github.com/powerline/fonts