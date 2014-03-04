---
layout: post
title: "OpsWorks Stack Host Listing and SSH Gem"
date: 2014-02-27 20:16
comments: true
published: false
categories: ospworks cloud ruby gem
---

### Making Life with OpsWorks Easier ###

Working in OpsWorks is has been a generally positive experience. There are a few things that I would like to see changed like the ability to position your own custom chef recipes _before_ OpsWorks built in recipes. Also there was a bit of a sorespot when we decided to automate running a few rake tasks from jenkins on a particular host in a particular stack. To solve this issue, I've created a Ruby Gem that has a dual purpose. In its current form it allows you to easily list all of your stacks, see the nodes in that stack with their IP's and also SSH directly to a host using its stack name and hsotname.

To give it a shot simply...
  gem install owssh

The code lives here...
  [git@github.com:phutchins/owssh.git]

The gem in use...
```bash

```
