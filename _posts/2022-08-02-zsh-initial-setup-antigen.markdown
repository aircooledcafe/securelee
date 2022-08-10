---
layout: post
title: Setting up ZSH on linux with antigen
date: 2022-08-02 20:00 +0100
tags: linux zsh antigen 
---

Here is my setup for my shell in linux, I use [antigen][antigen] to manage plugins and [Powerlevel10k][powerlevel10k] them by [Roman Perepelitsa][romkatv]. All commands assume you are in your home directory.  

First things first is to install some dependencies (some others that I always need installed too):  

`sudo apt install zsh git curl autojump fonts-powerline wget vim`  

(If your not on Ubuntu or Debian, you can follow instructions [here][powerline] for powerline fonts.)  
Remember to change your shell font ot a powerline one, I quite often use [MesloLGS][mesloregular] now instead of the bundled Powerline fonts noted. Microsoft also make a really nice powerline font now called Cascadia Code, available on [GitHub][cascadia].
<!--more-->  
Next up is to download the antigen script and added it to a file:  

`curl -L git.io/antigen > antigen.zsh`  

Then we need to create our zshrc file and add our configuration to it:  

`vim .zshrc`  

And add the following lines, the first two lines are for initiating antigen and pointing to ur antigen configuration file (this can be included within the .zshrc if you wish). The if statement is for pulling in my aliases from a file I add them too, this is of course optional.    

{% highlight conf %}
source /home/vm1/antigen.zsh     
antigen init ~/.antigenrc  
  
# add aliases from my alias file  
if [ -f ~/.zsh_aliases ]; then  
    . ~/.zsh_aliases  
fi  
{% endhighlight %}

Now we need to create our `antigenrc` configuration file (these can be added to `.zshrc` though I learned this trick on the SANS SEC275 course I just took), then add all your configuration options to the file, for me it just the plugins:  

`vim .antigenrc`

Add the following lines:

{% highlight conf %}
# Load the oh-my-zsh's library.  
antigen use oh-my-zsh  

# Bundles from the default repo (robbyrussell's oh-my-zsh).   
antigen bundle git  
antigen bundle pip  
antigen bundle command-not-found
antigen bundle docker
# use the j autojump directory function
antigen bundle autojump

# a plugin to remind me that I set up an alias for that command!
antigen bundle djui/alias-tips

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

I would recommend running zsh now to test everything is configured correctly and now typos, this will also take your through the powerlevel10k setup routine:  

`zsh`

If all of the above works fine and your happy, change the default shell to zsh:  

`chsh -s /usr/bin/zsh`   

Log out and log back in for changes to take effect, enjoy your fancy new shell.

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
[cascadia]: https://github.com/microsoft/cascadia-code