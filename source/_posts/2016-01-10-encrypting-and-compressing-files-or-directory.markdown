---
layout: post
title: "Encrypting and Compressing a Directory with Password"
date: 2016-01-10 07:16
published: false
comments: true
categories: linux
---

This explains how to encrypt a file or directory and compress it with a single password. This allows users on most any OS to extract and decrypt the file using unzip which is generally available on most platforms.

`7z a -tzip -p new_archive.zip "my_folder_name"`

When prompted, enter the strong password
