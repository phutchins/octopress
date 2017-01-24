---
layout: post
title: "Docker Tips and Tricks"
date: 2017-01-23 08:15
comments: true
categories:
---

## Programatically Getting Container Info

### IP's & Ports
Get all containers IP addresses with container name

for docker
```
docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq)
```

for docker compose
```
docker inspect -f '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $(docker ps -aq)
```

Alias for getting the docker VM IP
