---
layout: post
title: "Installing Octopress"
date: 2013-10-24 11:47
comments: true
published: true
categories: [devops]
---

After intalling my new Octopress blog I figured it might be appropriate to write a blog about installing Octopress! The process was fairly easy but I found that going into it I'd made a few assumptions that were a little bit misleading. After getting over a couple of mental speed bumps, tiny ones really, I'm up and running with a brand new blog!

Assumptions
===========
* Octopress is a package of software that I will unpack into a directory on my web server and I will browse to it to administrate the setup and create posts

This was completely wrong. Come to find, its a good bit neater than this. You unpack or checkout the source onto a *nix or osx machine, fill out a simple config file, then use rake to build and then publish your site locally or remotely. After it has been published, it is a static site.

* I will need to set up a database to hook Octopress into

After messing with Wordpress and Drupal for many years, I pretty much assume that every blog suite will need a DB. This is not the case. Since Octopress builds your blog at your command, you publish static content so there is no need for a DB or even logging into an interface via the web. You do all of your blogging hackerstyle from the command line!

Installation
============

Steps...
---
1. Install and Ruby
2. Install Octopress (really just checking out a git repo)
3. Configure Octopress
l. Configure your Web Server

Install Ruby
---
This step will differ greatly depending on what opreating system you're installing this on. Generally you might want to install RVM to manage your ruby versions and then install ruby using RVM. I am serving this off of a server running CentOS so it was a little different for me...

Here was my process

#### Remove old ruby
``` bash
$ yum remove ruby
```

#### Install dependencies
``` bash
$ yum groupinstall "Development Tools"
$ yum install zlib zlib-devel
$ yum install openssl-devel
$ wget http://pyyaml.org/download/libyaml/yaml-0.1.4.tar.gz
$ tar xzvf yaml-0.1.4.tar.gz
$ cd yaml-0.1.4
$ ./configure
$ make
$ make install
```

#### Install ruby
``` bash
$ wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p194.tar.gz
$ tar zxf ruby-1.9.3-p194.tar.gz
$ cd ruby-1.9.3-p194
$ ./configure
$ make
$ make install
```

#### Update rubygems
``` bash
$ gem update --system
$ gem install bundler
```

#### Test ruby and rubygems are working
``` bash
#Close shell and reopen for changes to take effect
$ruby -v
$gem --version
```

#### Rails
``` bash
$ yum install sqlite-devel
$ gem install rails
$ gem install sqlite3
```

## Install Octopress

If you installed RVM, it may ask you to trust the .rvmc file, you can savely say "yes" to this
``` bash
$ cd
$ git clone git://github.com/imathis/octopress.git octopress
$ cd octopress
$ gem install bundler
$ bundle install
$ rake install
```

## Configure Octopress
#####You need to edit a few configuration options for Octopress.

##### _config.yml
Edit this file and configure the first few lines to your liking. You can see below how I configured mine. There are more configuration options near the bottom of the file that you can explore.
``` bash
url: http://phutchins.com
title: Philip Hutchins
subtitle: Head in the cloud...
author: Philip Hutchins
simple_search: http://google.com/search
description:
```

##### Rakefile
Edit this file to configure the way your blog deploys
``` bash
## -- Rsync Deploy config -- ##
# Be sure your public key is listed in your server's ~/.ssh/authorized_keys file
ssh_user       = "phutchins@phutchins.com"
ssh_port       = "22"
document_root  = "/var/www/vhosts/phutchins.com/httpdocs/"
rsync_delete   = false
rsync_args     = ""  # Any extra arguments to pass to rsync
deploy_default = "rsync"
```

## Configure your Web Server

If you havent set up a site for this yet, you'll need to copy the default site and give it a name for your new site
``` bash
$ cd /etc/apache2/sites-available
$ sudo cp default mysite
```

Open your new configuration file with vim or your favorite text editor
``` bash
$ sudo vim mysite
```

Lines that need to be changed are labeled with comments above. /var/www is the default directory so you will need to change that to the directory where you want to host your Octopress public folder out of. You can either point directly to the public folder or use the rake deploy function of Octopress to push your site to the web sites /var/www dir. I recommend the second option.

``` bash
<VirtualHost *:80>
  ServerAdmin webmaster@localhost

  <!-- point the line below to Octopress's public folder -->
  DocumentRoot /path/to/octopress/public/folder
  <Directory />
      Options FollowSymLinks
      AllowOverride None
  </Directory>

  <!-- point the line below to Octopress's public folder -->
  <Directory /path/to/octopress/public/folder >
      Options Indexes FollowSymLinks MultiViews
      AllowOverride None
      Order allow,deny
      allow from all
  </Directory>

  ScriptAlias /cgi-bin/ /usr/lib/cgi-bin/
  <Directory "/usr/lib/cgi-bin">
      AllowOverride None
      Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
      Order allow,deny
      Allow from all
  </Directory>

  ErrorLog ${APACHE_LOG_DIR}/error.log

  # Possible values include: debug, info, notice, warn, error, crit,
  # alert, emerg.
  LogLevel warn

  CustomLog ${APACHE_LOG_DIR}/access.log combined

    Alias /doc/ "/usr/share/doc/"
    <Directory "/usr/share/doc/">
        Options Indexes MultiViews FollowSymLinks
        AllowOverride None
        Order deny,allow
        Deny from all
        Allow from 127.0.0.0/255.0.0.0 ::1/128
    </Directory>

</VirtualHost>
```

Lastly, you'll need to restart apache
``` bash
$ sudo service apache2 reload
```

All done! Now you should be able to browse to the URL or IP of your Apache server and view your site. At this point it will be pretty empty and bare so lets move on to making your first blog post.


Your first post
===============

As Octopress is based on Jekyll, it offers rake tasks to create posts and pages that have some basic metadata included in them. It also wil generate global categories for you ass you add them to your posts and pages.

#### Adding a Blog Post

To create a post you use rake. Just replace "My Blog Title" with your title.
``` bash
rake new_post["My Blog Title"]
```

This creates a file for your post in the source/_posts directory named with the date that you created it and a munged version of the title you used. By default, these are created as a [Markdown](http://daringfireball.net/projects/markdown/) files. Navigate to your newly created file and you will see something like this...
```bash
---
layout: post
title: "My Blog Title"
date: 2013-09-14 00:21
comments: true
categories: [Blog, Awesome]
---
```

Creating the post
-----------------

Once you have the post file created, you can begin to add your content below the last set of three dashes. You work with simple text but why stop there when you have [Markdown](http://daringfireball.net/projects/markdown/)??

Working with Markup
-------------------

Markdown lets you do a lot so I'll only cover a few simple things here.

##### Headers
You can create headers in a few ways.

The above header is created like this. You can add are remove #'s to change the size. A single # represents a H1 while three #'s represents an H3 and so on.
```
##### Headers
```

Underlining text with equals or dashes creates H1's and H2's respectively.
```
This is an H1
=============

This is an H2
-------------
```
It does not matter how many equals or dashes you use. Any number will work

##### Code Blocks
For a code block, you can indent lines with a tab or 4 spaces. You can also use ``` before and after your code. To specify the type of highlighting use something like this...

    ``` bash
    ls -altr
    ```

##### Links
To create a link, use the following syntax.
```
[Link Text](http://my.page.com)
```

For more information check out the [Markdown Documentation Page](http://daringfireball.net/projects/markdown/syntax).

Deploy your new post into production
------------------
To generate your blog, simply run `rake generate`. This will create the blog and all files required for it in the `public` folder.

Then to publish to the location you configured in `Rakefile`, run `rake deploy`. This will push your `public` folder to the configed location.

Managing your Content
=====================
There are a few ways that you can manage your blog. I'm current using a git repo to store the entire Octopress codebase. This allows you to not only edit and make updates from many locations but it also gives you the ability to use git as a way to publish. 

Overall thoughts...
===================
Octopress is pretty easy to use and great for someone that doesn't mind a little hacking. Even better for someone that likes a little hacking. :) If you're like me, you hate when gui editors change something that you've typed or created becuase it thinks it knows better than you do.
