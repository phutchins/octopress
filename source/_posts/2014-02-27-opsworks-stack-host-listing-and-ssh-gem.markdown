---
layout: post
title: "OpsWorks Stack Host Listing and SSH Gem"
date: 2014-02-27 20:16
comments: true
published: true
categories: ospworks cloud ruby gem
---

### Making Life with OpsWorks Easier ###

Working in OpsWorks is has been a generally positive experience. There are a few things that I would like to see changed like the ability to position your own custom chef recipes _before_ OpsWorks built in recipes. Also there was a bit of a sorespot when we decided to automate running a few rake tasks from jenkins on a particular host in a particular stack. To solve this issue, I've created a Ruby Gem that has a dual purpose. In its current form it allows you to easily list all of your stacks, see the nodes in that stack with their IP's and also SSH directly to a host using its stack name and hsotname.

To give it a shot simply...
  gem install owssh

The gem in use...
```bash
$ owssh describe qa
Getting data for Stack: qa
┏━━━━━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━┓
┃ Hostname             ┃       Public IP ┃      Private IP ┃ Type             ┃ Status   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ redis2               ┃  10.123.123.123 ┃  10.100.100.100 ┃ redis            ┃ online   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ rails-app3           ┃     50.30.20.10 ┃  10.100.100.100 ┃ rails-app        ┃ online   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ db-master2           ┃   100.10.10.123 ┃  10.100.100.100 ┃ db-master        ┃ online   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ resque1              ┃    12.12.123.12 ┃   10.10.100.100 ┃ resque           ┃ online   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ rails-app4           ┃     12.12.12.12 ┃   10.10.100.100 ┃ rails-app        ┃ online   ┃
┣━━━━━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━╊━━━━━━━━━━━━━━━━━━╊━━━━━━━━━━┫
┃ resqueplus1          ┃             N/A ┃             N/A ┃ resqueplus       ┃ stopped  ┃
┗━━━━━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━┛

$ owssh qa rails-app
Opening SSH connection to first of type 'rails-app' which is 'rails-app3'...
 This instance is managed with AWS OpsWorks.

   ######  OpsWorks Summary  ######
   Operating System: Ubuntu 12.04.4 LTS
   OpsWorks Instance: rails-app3
   OpsWorks Instance ID: [removed]
   OpsWorks Layers: Rails App Server
   OpsWorks Stack: QA
   EC2 Region: us-east-1
   EC2 Availability Zone: us-east-1d
   EC2 Instance ID: [removed]
   Public IP: 123.123.123.123
   Private IP: 123.123.123.123

 Visit http://aws.amazon.com/opsworks for more information.
Last login: Thu May  1 16:48:52 2014 from my.awesome.computer
phutchins@rails-app3:~$
```

### Gem & Documentation ###

The code lives here...
  [git@github.com:phutchins/owssh.git]

[OwSSH Documentation](http://phutchins.com/tools/owssh.markdown)
