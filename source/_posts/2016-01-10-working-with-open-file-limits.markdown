---
layout: post
title: "Working with open file limits"
date: 2016-01-10 07:16
published: false
comments: true
categories: linux
---

# Working with Open Files Limits in Linux

When you run into an issue where your root user has reached the maximum number of open files and you are not logged in to the box, it can be difficult to troubleshoot. One way to work around this is to leave a ssh session to the host open with a process manager like htop running. This allows you to see whats going on when you reach the limit, and also to kill a few processes so that you can open another connection into the host for troubleshooting.

You can start by listing the number of currently open files for the system and by user and compare that to the currently set maximums.

You can temporarily increase the maximum number of open files with ulimit -n [number]

## Listing open files

### By User

```
sudo lsof -u [username] | wc -l
```

### By User for All Users
```
#!/bin/bash
TOTAL=0
for USER in $( cut -d: -f1 /etc/passwd ); do
  OPENFILES=`lsof -u $USER  2> /dev/null | awk 'BEGIN { total = 0; } $4 ~ /^[0-9]/ { total += 1 } END { print total }'`
  if [ $OPENFILES -gt 0 ]; then
    echo $USER: $OPENFILES
    TOTAL=$(($OPENFILES + $TOTAL))
  fi
done
echo Total: $TOTAL
```

## Determining the Current Limits

### Hard Limits
List current hard limits for all resources
```
ulimit -Ha
```

List current soft limits for all resources
```
ulimit -a
```

## Setting File Limits

### Temporary
To change the
```
su - [user]
ulimit -n [max_number]
```

### Permanent
In Ubuntu you can edit `/etc/security/limits.conf` to change these limits. Uncomment any line that you want to change the limit for or add additional lines. Here we have set the hard limit for the root user to 100000.
```
#*               soft    core            0
root             hard    nofile          100000
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#ftp             -       chroot          /ftp
#@student        -       maxlogins       4
```

For the system wide limit, edit `/proc/sys/fs/file-max`

For the limits.conf file to have any effect, you must add the following to `/etc/pam.d/common-session*`

```
session required pam_limits.so
```
