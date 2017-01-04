---
layout: post
title: "Installing Insteon USB PLM with OpenHAB"
date: 2016-11-19 12:10
comments: true
categories:
---

## Install Dependencies

## Connect, Test & Configure PLM

### Connect the PLM
+ Plug the PLM into a USB port on your server/computer

### Find the PLM Device
Look in /dev for your serial USB device

```
philip@cube:/dev$ ls | grep USB
ttyUSB0
```

In my case it was ttyUSB0. If you unplug and re-connect the PLM it may show up as a different device, such as ttyUSB1 so make sure to check if things stop working after you reconnect it.

### Test the PLM
To test the PLM, we'll be using Insteon Terminal which you will get from GitHub.

Lets install the dependencies for the Insteon Terminal which we will use to test and ensure the connection between the PLM and your computer are working.

+ Install ant, default-jdk and librxtx-java
`sudo apt-get install ant default-jdk librxtx-java`

+ Add the user accessing the PLM device to the correct groups to allow permission
If the user accessing the PLM is `openhab` use the following. You will need to check the current owner of the /dev/[yourdevice] file and the /run/lock folder and change `dialout` and `lock` below appropriately.
```
usermod -a -G dialout openhab
usermod -a -G lock openhab
```

+ Reboot
I've found that I have to reboot in order to connect to the model properly. This should not be the case but it is the simplest solution for now. It's possible that there is a permissions issue, dependency loading issue or something along those lines that gets resolved by a reboot.

+ Clone the Insteon Terminal repo from GitHub
`git clone https://github.com/pfrommerd/insteon-terminal.git`

+ Copy the example config file and edit appropriately
```
$ cd insteon-terminal
$ cp init.py.example init.py
$ vi init.py # Or use your editor of choice here
$ # Follow the instructions in the config file to configure Insteon Terminal using the PLM device we found earlier

+ Launch Insteon Terminal
From the insteon-terminal folder, launch the terminal

```
./insteon-terminal
```

At this point you should see something like the following...
```
philip@cube:~/github/insteon-terminal$ ./insteon-terminal
Insteon Terminal
Connecting
Connected
Terminal ready!
```

If you do not see `Connected` and instead see something like this...
```
philip@cube:~/github/insteon-terminal$ ./insteon-terminal
Insteon Terminal
Connecting
gnu.io.NoSuchPortException
Terminal ready!
```

Continue to troubleshoot why you cannot connect to your PLM modem device

