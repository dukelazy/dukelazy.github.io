---
layout: post
title: "Minecraft server template"
date: 2015-01-29 0‏‎1:43:02 +0800
comments: true
categories: "Minecraft"
tags: ["游戏","server","script","minecraft"]
---

<!--more-->

``` bash Minecraft_server_template.txt http://hydra-media.cursecdn.com/minecraft-zh.gamepedia.com/a/a9/Minecraft_server_template.txt Minecraft_server_template.txt
#!/bin/bash

# http://hydra-media.cursecdn.com/minecraft-zh.gamepedia.com/a/a9/Minecraft_server_template.txt

################################################################################
## MAKE SURE THIS SCRIPT IS AVAILABLE IN YOUR $PATH.
################################################################################
# It's highly advisable to create a directory to hold all of your server files. 
# The jar will generate quite a few files upon first run and it would get pretty 
# messy if there are other files in the same directory.
################################################################################
## NEEDED INFORMATION
################################################################################
# This is the location for a Debian/Ubuntu java executable
javaexec=/usr/bin/java

# Set this to the directory containing the minecraft_server.jar
# Provide the full path from /
server_location=CHANGE ME BEFORE DOING ANYTHING


################################################################################
## RUNNING THE SERVER
################################################################################
# By cd'ing to the server location
# All server files will be created in this directory.
cd $server_location

# Run the Server
`$javaexec -Xmx1024M -Xms1024M -jar $server_location/minecraft_server.jar nogui`

# After killing off the server, you will be cd'ed back to the directory you were 
# at prior to running this script.
cd -
```
