---
layout: page
title: "Elasticsearch Cheat Sheet"
date: 2016-05-17 10:01
comments: true
categories: [cheatsheet, elasticsearch]
---

# General Health

## Get Cluster Health
```
curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'
```

# Indexes

## Troubleshooting
List all indexes with health
```
curl -XGET 'localhost:9200/_cat/indices'
```


# Templates

## Listing
List all existing templates
```
curl -XGET localhost:9200/_template/ | jq '.'
```

# Scaling from One to Many Gotchas

## Set `number_of_replicas`
After you add new nodes to your cluster, and before you remove any, you need to set the number of replicas to the number of nodes that you have to ensure that you have all of your data on all nodes.
```
curl -XPUT 'localhost:9200/logstash-*/_settings' -d '{ "number_of_replicas": 4 }'
```
