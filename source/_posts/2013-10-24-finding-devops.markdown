---
layout: post
title: "Installing Octopress"
date: 2013-10-24 11:47
comments: true
published: true
categories: [devops]
---

After intalling my new Octopress blog I figured it might be appropriate to write a blog about installing Octopress! The process was fairly easy but I found that going into it I'd made a few assumptions that were a little bit misleading. After getting over a couple of speed bumps, tiny ones really, I'm up and running with a brand new blog!

Assumptions
===========
* Octopress is a package of software that I will unpack into a directory on my web server and I will browse to it to administrate the setup and create posts

This was completely wrong. Come to find, its a good bit neater than this. You unpack or checkout the source onto a *nix or osx machine, fill out a simple config file, then use rake to build and then publish your site locally or remotely. After it has been published, it is a static site.

* I will need to set up a database to hook Octopress into

After messing with Wordpress and Drupal for many years, I pretty much assume that every blog suite will need a DB. This is not the case. Since Octopress biulds your blog at your command, you publish static content so there is no need for a DB or even logging into an interface via the web. You do all of your blogging hackerstyle from the command line!

Installation
============


Your first post
===============

Creating the post
-----------------

Working with Markup
-------------------



Managing your Content
=====================


Overall thoughts...
===================
