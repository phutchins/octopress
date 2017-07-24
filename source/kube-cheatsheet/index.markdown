---
layout: page
title: "Kubernetes Cheatsheet"
date: 2017-05-17 15:01
comments: true
categories: [cheatsheet, kubernetes]
---

## Resource Usage

### Cluster
View summary of usage for Nodes
`kubectl top nodes`

View Node CPU Requests
`kube describe nodes | grep -A 2 -e "^\\s*CPU Requests"`

### Pod
`kubectl top pods`

## Listing Resources

List pods and sort by restart count
`kube get pods --sort-by="{.status.containerStatuses[:1].restartCount}"`

List pods and sort by nodename
`kubectl get pods -o wide --sort-by="{.spec.nodeName}"`

