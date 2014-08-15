---
layout: post
title: "Compare Commits/Tags/Branches on GitHub"
date: 2014-08-15 11:06
comments: true
categories: git
---

GitHub previously had a feature in their GUI allowing you to compare two different commits, tags or branches. The shortcuts to this feature in the GUI have been removed for some reason but the ability to do this is still there.

This is the basic URL format for doing a compare
```
http://github.com/<USER>/<REPO>/compare/[<START>...]<END>
```

### Options
Some of the available options for compare are undocumented.
(append these changes to the end of the URL)

Ignore whitespace changes
```
?w=1
```
