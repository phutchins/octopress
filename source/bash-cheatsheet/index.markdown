---
layout: page
title: "Bash CheatSheet"
date: 2014-02-07 13:14
comments: true
categories: [bash, cheatsheet]
---

##  History and Replacement

Search history
    Ctrl+r (enter search)
    Ctrl+r (again to find previous occurrences)
    Enter to stop searching and dump what you found to prompt
    ESC to abort

Replace first occurrence of a word in last command
    ^findthis^replacewiththis^

Replace all occurrences of a word in last command
    !!:gs/findallofthese/replacewiththis/

Navigate history
    CTRL+p - Previous command
    CTRL+n - Next command
    ARROW KEYS - you can also use the arrow keys but the CTRL shortcuts keep you from having to remove your hands from the home keys

## Loops

  for i in *.json; do knife environment from file $i; done


### OSX Specific

## Networking

Show routing table
    netstat -nr
