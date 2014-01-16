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
