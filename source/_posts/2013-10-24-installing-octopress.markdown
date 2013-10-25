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
3. Configure your Web Server

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


Creating the post
-----------------

Working with Markup
-------------------

Deploy your new post into production
------------------


Managing your Content
=====================


Overall thoughts...
===================
