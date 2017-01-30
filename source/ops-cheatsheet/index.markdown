---
layout: page
title: "DevOps (Operations) Cheatsheet"
date: 2015-06-04 10:23
comments: true
sharing: true
footer: true
categories: [cheatsheet, linux]
---

## Determine open files (file descriptors) for all processes by a user
```bash
watch 'lsof -u bws | awk '\''{print $2}'\'' | sort | uniq -c | sort -n'
```

## Check iowait and disk stats
Use sar (which you will have had to install and enable previously) or...

```bash
iostat -xm 5
```
