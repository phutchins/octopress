---
layout: post
title: "Tracking Down the Chef-Server Missing Dependency Bug"
date: 2014-03-04 07:52
comments: true
categories: [chef, ruby]
---

## Bug Description
Currently there is a fairly agrivating bug in the free version of Opscode's (now called Chef to make it even easier to find on google) chef-server. This bug is exposed when a dependency of one of your cookbooks depends on a cookbook who's dependency is not met.

### Symptoms
+ Chef-server starts consuming 100% CPU
+ Chef-server becomes unresponsive for periods of time or until erchef is restarted

### What is causing this bug?
The way that I understand it is that the depsolver that existed in older versions of chef-server was removed and it was the piece that was keeping us from hanging upon unresolved dependencies.

### Needs Clarification
+ Is the bug triggered for missign dependencies at any depth into the dependency chain, or only second level and below?
+ Is the bug triggered when the dependency cookbook exists on the server but the version constraing is not met, or only when the cookbook does not exist on the chef-server at all?

## How to Track Down Missing Dependency
```bash
knife exec -E 'puts api.post("/environments/_default/cookbook_versions", "run_list" => ["base", "chefdm-ssl"])'
```

## What is the fix and when will it be pushed to a stable release?
The fix is to add the depsolver that was used in the old chef-server back with some tweaks and testing. This has been committed to master but has not been added to a stable release yet. As of the writing of this article, the fix has yet to be released. It will not be released in any of the 11.0.X releases but is planned to be added in 11.1.0. Huge thanks to Ho-Sheng for shedding some light on this topic!

## omnibus-chef-server
You can build chef-server from the repo that contains the fix from here.
<https://github.com/opscode/omnibus-chef-server/commit/06d37db491f0040621f844354b2599631fb62e6b>

There is some more documentation on building nightlies here.
<http://docs.opscode.com/api_omnitruck.html>

### Links
The main bug report and discussion thread are here.
<https://tickets.opscode.com/browse/CHEF-3921>

The main piece of code is located here.
<https://github.com/opscode/chef_objects/commit/a3133ced037d1e508ff18723ad9a6f2b94dea1ea>

This is where it gets pulled into the erchef binary.
<https://github.com/opscode/erchef/commit/316a09c0657fab6ff4eb2b9222ab84336a5f039a>
