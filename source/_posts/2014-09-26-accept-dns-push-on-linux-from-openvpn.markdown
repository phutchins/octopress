---
layout: post
title: "Accept DNS Push on Linux from OpenVPN"
date: 2014-09-26 08:06
comments: true
categories:
---

## DNS Push from OpenVPN
If you've set up an OpenVPN server for multiple OS tennants, you might have noticed that your OSX clients connect and receive their DNS setting from the server just fine. Your Linux clients however, if running resolvconf or openresolv may not work as easily. Luckily there is a simple and easy fix.

### The Fix
In the clients OpenVPN config file add the following...
``` bash
script-security 2
up /etc/openvpn/update-resolv-conf
down /etc/openvpn/update-resolv-conf
```

You can also add this on the server side configuration in the `Custom Directives` section.

Note however tho that if you put this on the server side, all of your clients will get this change and the file that we are referencing above may not exist on their machines which will cause problems.
