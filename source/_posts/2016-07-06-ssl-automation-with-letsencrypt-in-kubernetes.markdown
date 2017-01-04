---
layout: post
title: "SSL Automation with LetsEncrypt in Kubernetes"
date: 2016-07-06 14:38
comments: true
categories: kubernetes
---

# Problem
When deploying services to Kubernetes, a certificate has to be injected into the container via secret. It doesn't make sense to have each container renew it's own certificates as it's state can be wiped at any given time.

# Solution
Build a service within each Kubernetes namespace that handles renewing all certificates used in that namespace. This service would kick off the request to renew each cert at a predetermined interval. It would then accept all verification requests ( GET request to domain/.well-known/acme-challenge ) and respond as necessary. After being issued the new certificate, it would recreate the appropriate secret which contains that certificate and initiate a restart of any container or service necessary.

# Spec

## SSL Renewal Container
To automate the creation and renewal of certificates, we will need to create container with Letsencrypt to request creation or renewal of each certificate, Nginx to receive and confirm domain validation, and scripts to push the generated certificates to secrets in Kubernetes. This container will be deployed to Kubernetes as a daemonset and should run in each of your Kubernetes clusters.

### Container Creation & Setup
+ Nginx
+ LetsEncrypt (CertBot)

### Pushing Secrets
+ kubectl
+ Access?

### Restarting Services
+ kubectl

### Domain List Configuration

## SSL Ingress in Kubernetes
Previously to acieve using SSL/TLS in Kubernetes, we had to set up some sort of SSL/TLS termination proxy. With the addition of a few new features in Kubernetes 1.2 Ingress, we're able to do away with the proxy and allow Kubernetes to handle this task.


