---
layout: post
title: "Creating a proxy host on linux no additional software"
date: 2015-04-08 11:16
comments: true
categories:
---

It is fairly easy to create a linux proxy host that proxies traffic from other hosts that don't have direct access to the internet. This is a great and simple solution for keeping your backend workers off of the public internet to avoid attacks while at the same time allowing outbound traffic from them.

Here are the steps to configure this setup...

## Worker

### Configuring the worker that does not have direct access to the internet

#### DNS
+ Ensure that the host is using an externally resolvable IP address for DNS (this may not be needed in most cases)
    edit /etc/resolvconf/resolv.conf.d/base
    add...
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

+ Reload config files for DNS
```bash
$ sudo resolvconf -u
```

#### Networking

+ Change default gateway to IP address of proxy host
```bash
$ ip route del default
$ ip route add default via 192.168.3.1
```
### Making the settings persist through a reboot

#### Default Route Changes

+ On Ubuntu you would edit your interfaces file, `/etc/network/interfaces` and update your private network interface block to include the following...
```bash
up ip route del default
up ip route add default via 192.168.3.1
```

... it would then look something like this ...
```bash
auto eth2
iface eth2 inet static
  address 192.168.0.2
  netmask 255.255.255.0
  up ip route del default
  up ip route add default via [PROXY_IP_ADDRESS]
```

## Proxy Host

### Configuring the proxy host to allow the worker to proxy it's traffic through it

#### Networking

+ add iptables rules to enable nat and forwarding with masquerade

You can add them via command line...
```bash
*nat
iptables -t nat -A POSTROUTING -o [PUBLIC_INTERFACE] -j MASQUERADE
*filter
iptables -A FORWARD -i [PUBLIC_INTERFACE] -o [PRIVATE_INTERFACE] -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i [PRIVATE_INTERFACE] -o [PUBLIC_INTERFACE] -j ACCEPT
```
The static config that this produces looks a little different than the commands used via command line to create it.
You can use `$ iptables-save > iptables.rules` to dump your rules to a file called `iptables.rules`. You can then use this file to programatically load the rules upon boot.

OR

You can create an iptables file `/etc/iptables.rules` to load the rules from...
```bash
*nat
-A POSTROUTING -o [PUBLIC_INTERFACE] -j MASQUERADE
*filter
-A FORWARD -i [PUBLIC_INTERFACE] -o [PRIVATE_INTERFACE] -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i [PRIVATE_INTERFACE] -o [PUBLIC_INTERFACE] -j ACCEPT
```
...then use the following bash script to restore the rules (this will wipe any existing rules not existing in /etc/iptables.rules
```bash
#!/bin/sh
iptables-restore < /etc/iptables.rules
exit 0
```

+ Enable forwarding at the OS level in configs which will persist at reboot by editing /etc/sysctl.conf and uncommenting net.ipv4.ip_forward and setting it to 1
+ Enable forwarding at the OS level by running...
```bash
  echo 1 > /proc/sys/net/ipv4/ip_forward
```


To set up proxying of ssh connections through the proxy host to the backend workers do the following

+ add iptables rules to proxy the ssh traffic to the appropriate hosts (note that this goes under the *nat table, do not add another *nat line if one alraedy esists)

```bash
*nat
...
# Proxy SSH connections to badkend hosts
-A PREROUTING  -p tcp -m tcp -d [PROXY_HOST_PUBLIC_IP] --dport [EXT_SSH_PORT_1] -j DNAT --to-destination [BACKEND_WORKER_HOST_1_PRIV_NET_IP]:[BACKEND_WORKER_HOST_SSH_LISTEN_PORT]
-A PREROUTING  -p tcp -m tcp -d [PROXY_HOST_PUBLIC_IP] --dport [EXT_SSH_PORT_2] -j DNAT --to-destination [BACKEND_WORKER_HOST_2_PRIV_NET_IP]:[BACKEND_WORKER_HOST_SSH_LISTEN_PORT]
-A POSTROUTING -p tcp -m tcp -s [BACKEND_WORKER_HOST_1_PRIV_NET_IP] --sport [BACKEND_WORKER_HOST_SSH_LISTEN_PORT] -j SNAT --to-source [PROXY_HOST_PUBLIC_IP]
-A POSTROUTING -p tcp -m tcp -s [BACKEND_WORKER_HOST_2_PRIV_NET_IP] --sport [BACKEND_WORKER_HOST_SSH_LISTEN_PORT] -j SNAT --to-source [PROXY_HOST_PUBLIC_IP]
...
*filter
# Proxy SSH connections to backend hosts
-A FORWARD -m state -p tcp -i [PUBLIC_INTERFACE] -o [PRIVATE_INTERFACE] --state NEW,ESTABLISHED,RELATED -j ACCEPT
-A [PUBLIC_INTERFACE_NICKNAME] -p tcp -m tcp --dport [EXT_SSH_PORT_1] -j ACCEPT
-A [PUBLIC_INTERFACE_NICKNAME] -p tcp -m tcp --dport [EXT_SSH_PORT_2] -j ACCEPT
```

+ $PUBLIC_INTERFACE_NICKNAME - refers to `-A INPUT -i eth2 -j privnet` where the interface nickname would be `privnet` and eth2 would be the private interface

## Making the IPTables changes persist through reboot

On Ubuntu add the following bash script named `iptablesload` to `/etc/network/if-pre-up.d/` (this will wipe any existing rules not existing in /etc/iptables.rules)
```bash
#!/bin/sh
iptables-restore < /etc/iptables.rules
exit 0
```
