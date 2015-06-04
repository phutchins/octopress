---
layout: page
title: "ops-cheatsheet"
date: 2015-06-04 10:23
comments: true
sharing: true
footer: true
---

## Determine open files (file descriptors) for all processes by a user
```bash
watch 'lsof -u bws | awk '\''{print $2}'\'' | sort | uniq -c | sort -n'
```
