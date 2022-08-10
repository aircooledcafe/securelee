---
layout: post
title: Enable full right click context menu in Windows 11.
date: 2022-08-06 13:00
tags: windows 11 rightclick right click registry
---
To enable the full right click context menu in Windows 11, hat tip to [PC Pro][pcpro] a paper magazine I still get occasionally.  
Go to the registy key:  
`HKEY_CURRENT_USER\SOFTWARE\CLASSES\CLSID`
<!--more-->  
Create a new ket called:  
`{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}`  

Under that key create a key called:
`InprocServer32`  

Set `InprocServer32` value to blank and click OK.

Close registry editor and restart the explorer.exe process.

To restart explorer.exe from Command Line:

{% highlight bash %}
> taskkill /f /IM explorer.exe
> start explorer.exe
> exit
{% endhighlight %}

I have since discovered a PowerShell command to set the appropriate registry keys in one command, hat tip to [XDA Developers][xda]:

{% highlight powershell %}
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
{% endhighlight %}

[pcpro]: https://subscribe.pcpro.co.uk/
[xda]: https://www.xda-developers.com/