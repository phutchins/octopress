---
layout: post
title: "Finding UTF-8"
date: 2013-10-30 22:18
published: true
comments: true
categories: [linux]
---

Issues due to differences in file encoding can sometimes be elusive and frustrating. I've found a few little tricks to uncover where the issue exists and to fix the problem.

If you have an idea of which file the characters are, you can use cat to display all characters and adds a $ to the end of your lines so that you can confirm that there are no special characters hiding in each line.
```bash
cat -vet filename
```
The options...
```bash
    -e                       equivalent to -vE
    -E, --show-ends          display $ at end of each line
    -t                       equivalent to -vT
    -T, --show-tabs          display TAB characters as ^I
    -v, --show-nonprinting   use ^ and M- notation, except for LFD and TAB
```

Determine what character encoding is used by a file like so...
```bash
file -bi [filename]
```

Use VIM to change a file's encoding
```bash
set encoding=utf-8
set fileencoding=utf-8
```
You will only notice a difference if you add utf-8 characters. Most characters on the keyboard will create a unicode equivalent if you hold down the alt key.

Change a files encoding via athe command line
```bash
iconv -f ascii -t utf8 [filename] > [newfilename]
```
OR
```bash
recode UTF-8 [filename]
```
