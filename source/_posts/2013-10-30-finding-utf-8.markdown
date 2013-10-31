---
layout: post
title: "Finding UTF-8"
date: 2013-10-30 22:18
published: false
comments: true
categories: [linux]
---

Issues due to differences in file encoding can sometimes be elusive and frustrating. I've found a few little tricks to uncover where the issue exists and to fix the problem.

If you have an idea of which file the characters are, you can use cat to display all characters and adds a $ to the end of your lines so that you can confirm that there are no special characters hiding in each line.
```bash
cat -vet filename
```

