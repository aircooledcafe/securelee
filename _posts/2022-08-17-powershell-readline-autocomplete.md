---
layout: post
title: ZSH style autocomplete in PowerShell with PSReadline
date: 2022-08-16 08:00
---

I went looking for a way to get ZSH style AutoComplete where it shows commands from youor history as you type and you can complete with a tap of the right arrow. And I came a cross [PSReadline][psreadline], a powerful PowerSHell model that does autocomplete exactly how I wanted it and so much more. I am using it mostly for the autocomplete functionality but if you have a read through it's default profile, there are some great additional things it can do.
<!--more-->
To the installation and set up. First you need to ensure you Execution Policy is set to remote signed, so from an Administrative PowerShell:  
  
{% highlight powershell %}
set-executionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
{% endhighlight %}
  
Then you need to install the module with:  
  
{% highlight powershell %}
Install-Module PSReadLine
{% endhighlight %}  
  
Then we need to add some lines to your profile file (.ps1 file) should be in your `~/Documents/PowerShell` folder by default:  
  
{% highlight powershell %}
\# import the module to enable it
Import-Module PSReadline  

\# Shows navigable menu of all options when hitting Tab
Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete
\# enable up/down arrows for navigating through the history
Set-PSReadLineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadLineKeyHandler -Key DownArrow -Function HistorySearchForward
\# moves the cursor to the end of the autocompleted command (remove if you want the cursor to remain where the completion started from)
Set-PSReadLineOption -HistorySearchCursorMovesToEnd
\# enable zsh autocompletion like auto completion
Set-PSReadlineOption -PredictionSource History
{% endhighlight %}
  
By default this will pull history from the default location, which you can find by `Get-PSReadlineOption`, normally located at `%USERPROFILE%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine`.

[psreadline]: https://github.com/PowerShell/PSReadLine