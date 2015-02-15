---
layout: post
title: "How to easily and quickly clean up your Ubuntu /boot partition"
date: 2015-02-15 16:39
comments: true
categories: ['ubuntu', 'linux']
---

# Cleanup /boot

After a few kernel upgrades, the /boot partition can fill up quickly if yours is 100M like mine. It's quite painful to remove package by package to free up some space so that you can continue upgrading. Here's a helpful oneliner to clean up all unused kernel packages...

```bash
for i in `dpkg -l 'linux-*' | sed '/^ii/!d;/'"$(uname -r | sed "s/\(.*\)-\([^0-9]\+\)/\1/")"'/d;s/^[^ ]* [^ ]* \([^ ]*\).*/\1/;/[0-9]/!d'`; do sudo apt-get -y purge $i; done
```
