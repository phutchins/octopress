---
layout: post
title: "Configuring A Static IP Block from ATT"
date: 2014-02-16 08:56
comments: true
categories: [tomato, att, ip, networking]
---

So you've ordered a block of static IP's from AT&T for use at home. You're tired of forwarding a million custom external ports to non matching internal ports. You want to use them to point to various hosts, virtual machines, and devices behind AT&T's modem/router combo or your router which is behind their modem/ruoter.

### Obtain your IP block information
The first step to getting this all set up is knowing the correct information about the static IP block that you have been assigned. These are assigned in blocks of 8, 16, 32, etc... One thing to note is that 3 of the IP's out of your block will not be usable by you. Those 3 represent the network,  gateway, and broadcast addresses.


