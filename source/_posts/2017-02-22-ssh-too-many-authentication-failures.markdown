---
layout: post
title: "SSH Too Many Authentication Failures"
date: 2017-02-22 07:20
comments: true
categories:
---

SSH by default will always try all keys known by the agent in addition to any identity files. To keep this from happening, you can specify IdentitiesOnly=yes in addition to specifying a particular key to use when authenticating.

To compare...

This command:
`ssh -i ~/.ssh/id_rsa me@myserver hostname`

will give you Received disconnect from myserver: 2: Too many authentication failures for me

However, if you add IdentitiesOnly=yes like so...
`ssh -o IdentitiesOnly=yes -i ~/.ssh/id_rsa me@myserver hostname`

you will see...

`myserver`

### SSH Agent
Generally this is caused by having more than 5 keys loaded in ssh-agent.

You can remove keys by running `ssh-add -d ~/.ssh/[key]`
