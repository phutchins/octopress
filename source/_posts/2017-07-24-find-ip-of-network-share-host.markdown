---
layout: post
title: "Find IP of Network Share Host"
date: 2017-07-24 08:27
comments: true
categories:
---

Finding the IP address of a device on your network is generally an easy thing to do. This isn't always the case when the device is a network device or share using mDNS, also known as Bonjour on OSX.

If you know the network name of the device, you can use the `dscacheutil` to query the device and get it's IP address.

```
dscacheutil -q host -a name [network name].local
```

For example, I have a NAS on my network but the nas requires an older display connector that I generally don't keep around. Unfortunately it lives on a network to which we don't control the IP space. When the IP of the device changes, its not been so easy to find it's address. This is what I used to find it's IP.

```
$ dscacheutil -q host -a name storjnas.local
name: storjnas.local
ip_address: 10.150.50.50
ip_address: 10.150.50.50
```
