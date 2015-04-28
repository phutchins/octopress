---
layout: post
title: "Bash Tricks and Shortcuts"
date: 2015-04-12 09:03
comments: true
categories:
---

### Loops
Often times you need to run the same task in bash against a number of different arguments. Loops in bash can make this very quick and easy.

One of the simplest ways you can do this in a one liner is as follows
```bash
$ for i in one two three four; do echo $i; done
one
two
three
four
```

You can also predefine an array to use later like this
```bash
files=( "/tmp/file_one" "/tmp/file_two" "/tmp/file_three" )
for i in "${files[@]}"
do
  echo $i
done
```

Or, to do this on one line
```bash
$ files=("/tmp/file_one" "/tmp/file_two" "/tmp/file_three" ); for i in "${files[@]}"; do echo $i; done
/tmp/file_one
/tmp/file_two
/tmp/file_three
```

You can use ranges with `seq`
```bash
for year in $(seq 2000 2013); do echo $year; done
2000
2001
2002
2003
2004
2005
2006
2007
2008
2009
2010
2011
2012
2013
```

If you need a counter you could do something like this
```bash
#!/bin/bash
## declare an array variable
declare -a array=("one" "two" "three")

# get length of an array
arraylength=${#array[@]}

# use for loop read all values and indexes
for (( i=1; i<${arraylength}+1; i++ ));
do
  echo $i " / " ${arraylength} " : " ${array[$i-1]}
done
```

### File Permissions
There are a few shortcuts that make life easier when working with file and directory permissions. Here are a few.

When you want to recursively change permissions in a directory, you will want to change the file permissions separately from the directory permissions. You can accomplish this by using two different find commands piped to xargs as follows.
```bash
$ find * -type d -print0 | xargs -0 chmod 0755 # for directories
$ find . -type f -print0 | xargs -0 chmod 0644 # for files
```
or
```bash
$ find /path/to/directory -type d -exec chmod g+rsx '{}' \;
$ find /path/to/files -type f -exec chmod g+rsx '{}' \;
```

Three permission triads
```
first triad       what the owner can do
second triad      what the group members can do
third triad       what other users can do
```
Each triad
```
first character   r: readable
second character  w: writable
third character   x: executable
                  s or t: executable and setuid/setgid/sticky
                  S or T: setuid/setgid or sticky, but not executable
```

#### References, Operators and Modifiers
Above, you can see that permissions can be changed using u, g, o and a. These represent references to User, Group, Other and All.
+ (u)ser:
  + The user is the owner of the files. The user of a file or directory can be changed with the chown [3]. command.
  + Read, write and execute privileges are individually set for the user with 0400, 0200 and 0100 respectively. Combinations can be applied as necessary eg: 0700 is read, write and execute for the user.
+ (g)roup:
  + A group is the set of people that are able to interact with that file. The group set on a file or directory can be changed with the chgrp [4]. command.
  + Read, write and execute privileges are individually set for the group with 0040, 0020 and 0010 respectively. Combinations can be applied as necessary eg: 0070 is read, write and execute for the group.
+ (o)ther:
  + Represents everyone who isn't an owner or a member of the group associated with that resource. Other is often referred to as "world", "everyone" etc.
  + Read, write and execute privileges are individually set for the other with 0004, 0002 and 0001 respectively. Combinations can be applied as necessary eg: 0007 is read, write and execute for other.
+ (a)ll:
  + Represents everyone


The operator is what is used to control adding or removing of modifiers
+ + Add the specified file mode bits to the existing file mode bits of each file
+ - removes the specified file mode bits to the existing file mode bits of each file
+ = adds the specified bits and removes unspecified bits, except the setuid and setgid bits set for directories, unless explicitly specified.


Modifiers
+ r read
+ w write
+ x execute (or search for directories)
+ X execute/search only if the file is a directory or already has execute bit set for some user
+ s setuid or setgid (depending on the specified references)
+ S setuid or setgid (depending on the specified references) without the executable bit (or search for directories) set
+ t restricted deletion flag or sticky bit

#### Octal

+ The read bit adds 4 to its total (in binary 100),
+ The write bit adds 2 to its total (in binary 010), and
+ The execute bit adds 1 to its total (in binary 001).

These values never produce ambiguous combinations; each sum represents a specific set of permissions. More technically, this is an octal representation of a bit field – each bit references a separate permission, and grouping 3 bits at a time in octal corresponds to grouping these permissions by user, group, and others.


#### SetUID, SetGID and the Stick Bit
SUID / Set User ID : A program is executed with the file owner's permissions (rather than with the permissions of the user who executes it).

```bash
$ chmod  u+s testfile.txt
$ chmod 4750  testfile.txt
```

SGID / Set Group ID : Files created in the directory inherit its GID, i.e When a directory is shared between the users , and sgid is implemented on that shared directory , when these users creates  directory, then the created directory has the same gid or group owner of its parent directory.

```bash
$ chmod g+s
$ chmod 2750
```

Sticky Bit :  It is used mainly used on folders in order to avoid deletion of a folder and its content by other user though he/she is having write permissions. If Sticky bit is enabled on a folder, the folder is deleted by only owner of the folder and super user(root). This is a security measure to suppress deletion of critical folders where it is having full permissions by others.

```bash
$ chmod o+t /opt/ftp-data
$ chmod +t /opt/ftp-data
$ chmod 1757 /opt/ftp-dta
```

'S' = The directory's setgid bit is set, but the execute bit isn't set.
's' = The directory's setgid bit is set, and the execute bit is set.


These are represented in the `ls -la` (list all files in list format) by the following
```
Permissions Meaning
--S------   SUID is set, but user (owner) execute is not set.
--s------   SUID and user execute are both set.
-----S---   SGID is set, but group execute is not set.
-----s---   SGID and group execute are both set.
--------T   Sticky bit is set, bot other execute is not set.
--------t   Sticky bit and other execute are both set.
```
