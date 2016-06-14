---
layout: post
title: "Chef Provisioner SSL Errors"
date: 2016-05-03 14:23
comments: true
categories:
---

While setting up chef-provisioning to provision servers in Google Cloud, I ran into a pretty tricky bug which took a number of hours to troubleshoot.

## Command I Was Running
`chef-client -z elasticsearch-cluster.rb`

## The error...
```
    Compiled Resource:
    ------------------
    # Declared in /Users/philip/github/chef-storj/provisioners/elasticsearch-cluster.rb:21:in `from_file'

    machine("elasticsearch-1") do
      action [:converge]
      retries 0
      retry_delay 2
      default_guard_interpreter :default
      chef_server {:chef_server_url=>"http://localhost:8889", :options=>{:api_version=>"0"}}
      driver "fog:Google"
      machine_options {:insert_options=>{:tags=>{:items=>["elasticsearch"]}, :disks=>[{:deviceName=>"elasticsearch-1", :autoDelete=>true, :boot=>true, :initializeParams=>{:sourceImage=>"projects/ubuntu-os-cloud/global/images/ubuntu-1404-trusty-v20150316", :diskType=>"zones/us-east1-b/diskTypes/pd-ssd", :diskSizeGb=>80}}, {:type=>"PERSISTENT", :mode=>"READ_WRITE", :zone=>"zones/us-east1-b", :source=>"zones/us-east1-b/disks/elasticsearch-1", :deviceName=>"elasticsearch-1"}]}, :key_name=>"google_default"}
      declared_type :machine
      cookbook_name "@recipe_files"
      recipe_name "/Users/philip/github/chef-storj/provisioners/elasticsearch-cluster.rb"
      run_list ["recipe[chefsj-elk::elasticsearch-1]"]
    end

[2016-05-03T13:26:18-04:00] INFO: Running queued delayed notifications before re-raising exception

Running handlers:
[2016-05-03T13:26:18-04:00] ERROR: Running exception handlers
Running handlers complete
[2016-05-03T13:26:18-04:00] ERROR: Exception handlers complete
Chef Client failed. 0 resources updated in 04 seconds
[2016-05-03T13:26:18-04:00] FATAL: Stacktrace dumped to /Users/philip/.chef/local-mode-cache/cache/chef-stacktrace.out
[2016-05-03T13:26:18-04:00] FATAL: Please provide the contents of the stacktrace.out file if you file a bug report
[2016-05-03T13:26:18-04:00] ERROR: machine[elasticsearch-1] (@recipe_files::/Users/philip/github/chef-storj/provisioners/elasticsearch-cluster.rb line 21) had an error: Faraday::SSLError: SSL_connect returned=1 errno=0 state=SSLv3 read server certificate B: certificate verify failed
[2016-05-03T13:26:19-04:00] FATAL: Chef::Exceptions::ChildConvergeError: Chef run process exited unsuccessfully (exit code 1)
```

## Testing SSL
Using the `knife ssl check` command, check the status of ssl between you and your chef server.

## Obtaining an Updated cert.pem
```
curl http://curl.haxx.se/ca/cacert.pem -o /usr/local/etc/openssl/cert.pem
```

## The Problem
The precompiled versions of ruby from RVM are pointing at G`/etc/openssl/certs` when looking for it's ca certificate file. Newer versions of OSX have moved their certs to a different directory, or possibly `/usr/local/etc/openssl/certs` if you've installed openssl from brew or some other source.


## The Solution
Reinstall ruby from source.
`rvm reinstall 2.2.1 --disable-binary`

Uninstall all the chef gems
`gem uninstall chef chef-zero berkshelf knife-solo`

Reinstall ChefDK


## Links
+ [Failing SSL Verification with RVM](https://toadle.me/2015/04/16/fixing-failing-ssl-verification-with-rvm.html)
+ [SSL Tools](https://github.com/mislav/ssl-tools)
+ [Stack Overflow Post on Certificate Verify Failed](http://stackoverflow.com/questions/8101377/certificate-verify-failed-openssl-error-when-using-ruby-1-9-3)
