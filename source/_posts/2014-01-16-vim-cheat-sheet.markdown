---
layout: post
title: "VIM Cheat Sheet"
date: 2014-01-16 15:01
comments: true
categories: VIM
---

### Comment mutiple lines
Select your lines with a VISUAL BLOCK
	Ctrl + v
Then use capital insert to insert a character before every line
	Shift + I

Also, you can select your lines with VISUAL LINE
	Shift + V
And then use regex to munge each of the lines. This example substitutes the start of each line with #.
	s/^/#/

### Visual Mode & Copy Paste
  Copy entire file
    gg - Go to beginning of file
    "*y - Starts a yank command to the register * from the first line, until...
    G - go to the end of the file

### Navigation
```bash
       Command         Function
       h               Move one character LEFT
       j               Move one character DOWN
       k               Move one character UP
       l               Move one character RIGHT

       b               Move BACK one word
       f               Move FORWARD one word

       gg              beginning of file
       G               end of file
```

### Manipulation
```bash
    Ctrl-u            Cut all BEFORE cursor
    Ctrl-k            Cut all AFTER cursor
```

## Split & Diff
  Open two files in diff mode
    vim -d file.one file.two

  Commands while in diff mode
```bash
    Ctrl-w Ctrl-w         Switch windows
    Ctrl-w h              Move to window LEFT
    Ctrl-w j              Move to window DOWN
    Ctrl-w k              Move to window UP
    Ctrl-w l              Move to window RIGHT

    Ctrl-w t              Move to the TOP window
    Ctrl-w b              Move to the BOTTOM window

    Ctrl-w H              Move window to the far LEFT
    Ctrl-w J              Move window to the BOTTOM
    Ctrl-w k              Move window to the TOP
    Ctrl-w L              Move window to the far RIGHT

    Ctrl-w =              Make both windows equal after terminal resize
```
