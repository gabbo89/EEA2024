---
layout: default
title: Use of the terminal
nav_order: 2
parent: 1. Introduction
description: A comprehensive guide to understanding epigenetics.
published: true
---

The analyses will be performed on a remote server, which can be accessed remotely from any computer with an internet connection. In order to connect to the server a software with SSH (Secure Shell) capabilities is required, that enable the user to interact with the server as if they were physically present. 

For **windows** user's Mobaxterm is the client reccomended, while for **mac** and **linux** users the terminal is the default option.

The ip address is: 158.110.36.60 
The username is the one already used 

- **Windows**

Mobaxterm can be downloaded freely (home edition) from the official website: https://mobaxterm.mobatek.net/download.html. 

![alt text](image.png)

Once downloaded, the software can be installed following the instructions on screen. 

Define how to set the ssh connection !!

Files can be graphically accessed using the panel on the left side on Mobaxterm. The user can navigate through the directories and select the files they want to use. The files can be copied and pasted to the local computer.

![mobax_panel](image-1.png)


- **mac**
To connect to the server and be able to use interactive and graphical softwares, you need first to have installed on your computer the XQuartz software, that can be downloaded from the official website: https://www.xquartz.org/.

Next I reccomend to connect from the terminal to the server using the following syntax:

`ssh -X {username}@{srv_name}`

`-X` enable the use of interactive devices that use X11 protocol. 

In addition with very new mac devices you may have problems with the XQuartz software, in this case you can add the following command line to your `~/.bashrc` file:

`export _JAVA_OPTIONS='-Dsun.java2d.xrender=false -Dsun.java2d.pmoffscreen=false'`

This should solve the problem of black windows (on igv) and other graphical issues.

For any concern, just tell me!

In order to navigate graphically through the directories and select the files you want to use on the server, you can use a client FTP, like Cyberduck (https://cyberduck.io/). The are several available and alternatives softwares available (for example filezilla).

You will need to set the server address, the username and the password.