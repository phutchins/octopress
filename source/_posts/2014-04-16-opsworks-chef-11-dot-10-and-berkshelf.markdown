---
layout: post
title: "Opsworks Chef 11.10 and Berkshelf"
date: 2014-04-16 07:46
comments: false
published: false
categories: chef
---

## Chef Search
Opsworks with Chef 11.10 now supports chef search. It does this using chef-zero so it only supports data found locally in a few locations.

### Sources for Chef Search Data
+ Attributes from an instances stack config and deployment JSON
+ Attributes from an instances built-in and custom cookbooks attributes files
+ System data that is collected by Ohai

### Example of how to use chef search on Opsworks
```ruby
appserver = search(:node, "role:php-app").first
Chef::Log.info("The Private IP is '#{appserver[:private_ip]}'")
```

## Data Bags

### Example of how to access data bags on Opsworks
```ruby
mything = data_bag_item("myapp", "mysql")
Chef::Log.info("The username is '#{mything['username']}' ")
```

## Berkshelf on Opsworks


## Sources
Amazon AWS Opsworks Documentation
<http://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-chef11-10.html>
