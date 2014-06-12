---
layout: page
title: "Handy Shortcuts"
date: 2014-05-02 08:42
comments: true
sharing: true
footer: true
categories: [cheatsheet]
---

## OSX
Ctrl + Shift + h - Visual list of history in iTerm2

## Linux

### Command Line
Iterate through all files in a directory and split the names to be used also as directory name
```bash
for file in *; do echo "Checking $file"; diff ./$file ~/github/backseat/widgets/$(echo ${file//.*/ })/$file; done
```

#### Constrain Grep Line Output
Grep and return X (40 in this case) characters before and after result per line
This helps to avoid having grep results that return extremely long lines which makes it difficult to parse the output.
```bash
egrep -Rso '.{0,40}wna4{0,40}' ./*
```

## VI
Save a file using sudo when you currently do not currently have permissions
```bash
$:%!sudo tee %
```
