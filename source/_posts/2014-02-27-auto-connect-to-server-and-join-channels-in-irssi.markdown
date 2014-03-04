---
layout: post
title: "Auto Connect to server and join channels in irssi"
date: 2014-02-27 20:04
comments: true
categories: irc
---

irssi is a wonderful chat client. I'm closing it and reopening it all of the time so its nice when it automatically connects to the server that you want, identifies your nickname and joins your channels. Here is how to do it.

### How to configure irssi to auto connect to a server ###

Add the server or servers that you want to connect to
    /SERVER ADD -auto -network freenode irc.freenode.net 6667

### Set and Identify nickname ###

If you want to automatically set and identify your nickname with nickserv you can do it like this...
    /NETWORK ADD -autosendcmd "/^nick phutchins;/^msg nickserv identify mysecurepassword;wait 2000" freenode

### Automatically join certain channels upon connection to a specific network ###

To join a channel
    /CHANNEL ADD -auto #powerline freenode

To join a channel with a password
    /CHANNEL ADD -auto #awesomesecretchannel freenode supersecurepass
