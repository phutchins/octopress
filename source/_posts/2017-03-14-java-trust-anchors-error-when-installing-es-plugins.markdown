---
layout: post
title: "Java trust anchors error when installing ES plugins"
date: 2017-03-14 18:14
comments: true
categories:
---

## The Error
```
root@elasticsearch-1:/opt/elasticsearch/elasticsearch# ./bin/elasticsearch-plugin install repository-gcs
-> Downloading repository-gcs from elastic
Exception in thread "main" javax.net.ssl.SSLException: java.lang.RuntimeException: Unexpected error: java.security.InvalidAlgorithmParameterException: the trustAnchors parameter must be non-empty
        at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
          at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
          at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
          at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
          at sun.net.www.protocol.http.HttpURLConnection$10.run(HttpURLConnection.java:1926)
          at sun.net.www.protocol.http.HttpURLConnection$10.run(HttpURLConnection.java:1921)
          at java.security.AccessController.doPrivileged(Native Method)
          at sun.net.www.protocol.http.HttpURLConnection.getChainedException(HttpURLConnection.java:1920)
          at sun.net.www.protocol.http.HttpURLConnection.getInputStream0(HttpURLConnection.java:1490)
          at sun.net.www.protocol.http.HttpURLConnection.getInputStream(HttpURLConnection.java:1474)
          at sun.net.www.protocol.https.HttpsURLConnectionImpl.getInputStream(HttpsURLConnectionImpl.java:254)
          at org.elasticsearch.plugins.InstallPluginCommand.downloadZip(InstallPluginCommand.java:279)
          at org.elasticsearch.plugins.InstallPluginCommand.downloadZipAndChecksum(InstallPluginCommand.java:322)
          at org.elasticsearch.plugins.InstallPluginCommand.download(InstallPluginCommand.java:231)
          at org.elasticsearch.plugins.InstallPluginCommand.execute(InstallPluginCommand.java:210)
          at org.elasticsearch.plugins.InstallPluginCommand.execute(InstallPluginCommand.java:195)
          at org.elasticsearch.cli.SettingCommand.execute(SettingCommand.java:54)
          at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:122)
          at org.elasticsearch.cli.MultiCommand.execute(MultiCommand.java:69)
          at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:122)
          at org.elasticsearch.cli.Command.main(Command.java:88)
          at org.elasticsearch.plugins.PluginCli.main(PluginCli.java:47)
```

## The Reason
  The message means that the trust store that you specified or was specified for you could not be opened due to access, permissions, or due to the fact that it doesn't exist.

  To fix this, you need the ca-certificates-java package which is not explicitly installed by the Oracle JDK/JRE. Also, it may be installed but you still have to manually run the configuration for it.

## The Solution
  The solution is to run the configuration for this package. Make sure to install the package if it hasn't already been installed.

  ```
  sudo /var/lib/dpkg/info/ca-certificates-java.postinst configure
  ```
