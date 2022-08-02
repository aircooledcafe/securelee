---
layout: post
title:  "Using CMD to create individual archives of multiple folders."
date:   2022-08-02 14:14 +0100
tags: windows CMD 7zip command line
---


Using the command line in Windows to create and individual zip archive of multiple folders.

Use case I have a directory with:  
`folder1`      
`folder2`  
`folder3`   

Which I want to make into:  
`folder1.zip`  
`folder2.zip`  
`folder3.zip`  

You can use the following command, I am using 7zip replace with your archive tool of choice. This will create a zip in the root folder.

`for /D %d in (.) do "C:\Program Files\7-Zip\7z.exe" a -tzip "%d.zip" ".%d*"`

Break down of command:
`for` each directory `%d` is the variable and `/D` defines we're looking at directories.  
`(*.*)` defines the search location, root of folder your in.  
`do "C:\Program Files\7-Zip\7z.exe" a -tzip` - 7zip command to add to a zip archive.  
`"%%d.zip"` path of where you want the archive to be located.  
`".%%d*"` defines the naming convention within the zip, this will have no subdirectory within the zip