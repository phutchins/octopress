---
layout: post
title: "Debugging SSH Connection Issues"
date: 2014-09-02 14:09
comments: true
published: true
categories: [ssh, linux]
---

Debugging SSH connection issues can be tricky and frustrating.

### Common Issues

+ ssh_exchange_identification: Connection closed by remote host

### Debugging Steps

+ Run ssh in debug mode
``` bash
ssh my.host.com -VVV
```
+ Watch logs on remote server (if possible)
+ Run sshd on separate port with debug logging to console
``` bash
/bin/sbin/sshd -p 24 -D -d -e
```
####Explination
``` bash
-p    Set the listening port.
-D    This option tells sshd to not detach and does not become a daemon. It allows for easy monitoring.
-d    Enable debug mode.
-e    Write logs to standard error instead of system log.
```
