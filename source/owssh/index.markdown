---
layout: page
title: "owSSH Documentation"
date: 2014-03-04 11:30
comments: true
sharing: true
footer: true
categories: [tools, ruby, gem, opsworks, aws]
---
**owSSH** is a Ruby gem that wraps awscli and allows you to quickly and easily list all stacks, stack details and ssh to nodes within a stack in OpsWorks. It provides a executable that gives you a command line tool called owssh and also provides a library that returns usable objects containing stack information.


## Prerequisites
+ awscli - http://aws.amazon.com/cli/ - Installed automatically upon gem installation
+ command_line_reporter - https://rubygems.org/gems/command_line_reporter


## Installation
+ Ensure that you have the prerequisites installed
+ Install the owssh gem
    `gem install owssh`


## Configuration
+ Owssh defaults to using the aws config file below
    + `~/.aws/config.owssh`
    + You can override this by setting the following AWS environment variable
      + `AWS_CONFIG_FILE`
    + Or if you do not want to use the same config that awscli is using, set the following
      + `OWSSH_AWS_CONFIG_FILE`


+ Have your SSH key in the correct location and with the correct name and permissions (the name of this key file will be moved to a config file in the future)
    + `cp my_id_rsa ~/.ssh/id_rsa_owssh`
      +`chmod 600 ~/.ssh/id_rsa_owssh`
    + You can override this by setting the following environment variable
      +`OWSSH_SSH_KEY_FILE`


+ Owssh will use the `default` aws profile unless you override it with the following environment variable
    + `AWS_DEFAULT_PROFILE`


##Command Line Usage
    Version 0.0.21

    Usage:
    owssh list                                                   - List all environments
    owssh describe                                               - Show details of hosts in all stacks
    owssh describe [Stack Name]                                  - Show details of a specific stack
    owssh [Stack Name] [Hostname or Type]                        - SSH to a host in a stack
    owssh [Stack Name] [Hostname or Type] "Your command here"  - SSH to a host in a stack and run a command

     Type      - The type of host. I.E. rails-app, resque, etc...
      Hostname  - The name of the host. I.E. rails-app1, resque1, etc...


##Library Usage
    HASH get_stacks                      - Returns a hash with key of the Stack Name and Value of the Stack ID
    HASH get_instances                   - Returns a hash with key of Hostname and value of a hash containing the following...
                                            "PUB_IP"  => Instance Public IP
                                            "PRIV_IP" => Instance Private IP
                                            "TYPE"    => Type of Instance
    NIL  print_instances(instances_hash) - Prints a list of instances
    NIL  print_stacks(stacks_hash)       - Prints a list of stacks
