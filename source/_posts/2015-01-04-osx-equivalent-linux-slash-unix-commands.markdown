---
layout: post
title: "OSX Equivalent Linux/UNIX Commands"
date: 2015-01-04 10:59
comments: true
categories: [linux, osx]
---

There are a few staple commands that we use as engineers to trobleshoot issues on Linux machines and servers. Some of these unfortunately do not translate directly to OSX's underlying UNIX based system. Fortunately three are equivalent commands for most of them!

## Networking
It is very handy to be able to determine what ports are listening on a box, or not. It's also helpful to be able to determine which process and binary is using that port.

### List open ports

#### Linux
```bash
$ netstat -antlp
(snippet of output)
tcp        0      0 10.210.149.179:37499    10.210.149.179:21041    TIME_WAIT   -
tcp        0      0 10.210.149.179:36687    10.210.149.179:21041    TIME_WAIT   -
tcp        0      0 10.210.149.179:37499    10.210.149.179:21041    TIME_WAIT   -
tcp6       0      0 :::14150                :::*                    LISTEN      14765/zabbix_agentd
tcp6       0      0 :::21030                :::*                    LISTEN      15751/bitcoind
tcp6       0      0 :::21031                :::*                    LISTEN      15751/bitcoind
```

#### OSX
```bash
$ lsof -i -P -n
(snippet of output)
Bitcoin-Q  5114 phutchins   57u  IPv4 0x769f1abf3f7392c3      0t0  TCP 10.0.0.13:59997->188.138.104.253:8333 (ESTABLISHED)
Bitcoin-Q  5114 phutchins   62u  IPv4 0x769f1abf24ef47a3      0t0  UDP *:*
Bitcoin-Q  5114 phutchins   63u  IPv4 0x769f1abf46995463      0t0  TCP 10.0.0.13:59354->185.53.131.187:8333 (ESTABLISHED)
node       5115 phutchins   15u  IPv4 0x769f1abf251ad853      0t0  TCP *:9000 (LISTEN)
node       5115 phutchins   16u  IPv4 0x769f1abf251ae123      0t0  TCP 127.0.0.1:52442->127.0.0.1:27017 (ESTABLISHED)
node       5115 phutchins   17u  IPv4 0x769f1abf2516f123      0t0  TCP 127.0.0.1:52443->127.0.0.1:27017 (ESTABLISHED)
node       5115 phutchins   18u  IPv4 0x769f1abf2509a2c3      0t0  TCP 127.0.0.1:52444->127.0.0.1:27017 (ESTABLISHED)
```

### List routes

#### Linux
```bash
$ route -n
```

#### OSX
```bash
netstat -nr
```
