---
layout: post
title: Enable full right click menu in Windows 11.
date: 2022-08-06 13:00
tags: windows 11 rightclick right click registry
---
To enable the full right click menu in Windows 11.  
Go to the registy key:  
`HKEY_CURRENT_USER\SOFTWARE\CLASSES\CLSID`

Create a new ket called:  
`{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}`  

Under that key create a key called:
`InprocServer32`  

Set `InprocServer32` value to blank and click OK.

Close registry editor and restart the explorer.exe process.

TO restart explorer.exe from Command Line:

{% highlight bash %}
taskkill /f /IM explorer.exe
start explorer.exe
exit
{% endhighlight %}