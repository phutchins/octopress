---
layout: post
title: "Debugging SSH Connection Issues"
date: 2014-09-02 14:09
comments: true
published: false
categories: [ssh, linux]
---

Debugging SSH connection issues can be tricky and frustrating.

## Common Issues & Causes

+ ssh_exchange_identification: Connection closed by remote host
  + SSHD key's are corrupt
  + Connection to host does not complete due to network issue
  + The signature for the remote host in `known_hosts` is not correct
  + There is a problem with the SSH Daemon on the remote host

## Debugging Steps

1. Check `hosts.deny` and `hosts.allow` and ensure that you are not blocking the client, or allowing the client if necessary
2. Check `MaxStartups` value in `/etc/ssh/sshd_config`, the default is `10` but something like `10:30:60` is a bit safer for ssh brute force attacks
3. Run ssh in debug mode.
..+ This will help to expose problems with things like keys and auth types.

``` bash
ssh my.host.com -VVV
```

4. Watch logs on remote server (if possible)
5. Run sshd on separate port with debug logging to console
..+ This is a very useful step. After you start the ssh daemon on the remote host (we use a different port so that we can troubleshoot while being remote). The high port number allows you to run the sshd process as a non root user.
``` bash
/usr/sbin/sshd -p 2121 -D -d -e
```
####Explanation
```
-p    Set the listening port.
-D    This option tells sshd to not detach and does not become a daemon. It allows for easy monitoring.
-d    Enable debug mode.
-e    Write logs to standard error instead of system log.
```
