---
layout: post
title: "Elasticsearch Notes"
date: 2016-09-15 08:54
comments: true
categories: elasticsearch
---

Elasticsarch is a wonderful and powerful search and analytics engine. Operating an ES cluster or node is generally fairly easy and straight forward however there are a few situations where the resolution to seemingly common issues is not so clear. I will gather my notes and helper scripts here in an effort to help others better understand and resolve these certain issues and configurations quickly.

## Stats
Determine the amount of space on each node and other storage related stats
```
curl -XGET 'http://localhost:9200/_nodes/stats' | jq '.nodes [] | .name, .fs'
```

## Settings
### Set number of replicas for all indices to 0
When you spin up a single node cluster, the default setting for number of replicas is 1. This means that the cluster is going to try to create a second copy of each shard. This is not possible as you only have one node in the cluster. This keeps your cluster (single node) in the yellow status and it will never reach green. A node can function this way but it is annoying to not see a green state when everything is actually healthy.

```
curl -XPUT 'localhost:9200/_settings' -d '{"index": { "number_of_replicas": 0 } }'
```

## Recovering
### Ran out of disk
When you run out of disk, shards will have not been allocated and your cluster will likely be stuck in status RED. To recover, you need to find out which indices are unassigned and assign them manually

##### Commands

Check your clusters health and status of unassigned shards
```
curl -XGET http://localhost:9200/_cluster/health?pretty=true
```

Display the indices health
```
curl -XGET 'http://localhost:9200/_cluster/health?level=indices&pretty'
```

Display shards
```
curl -XGET 'http://localhost:9200/_cat/shards'
```

Display all unassigned shards and reason for being unassigned
```
curl -XGET localhost:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason| grep UNASSIGNED
```

or

```
curl -XGET http://localhost:9200/_cluster/allocation/explain | jq '.'
```

## Moving, canceling, and allocating shards (5.5)
```
 curl -XPOST http://localhost:9200/_cluster/reroute -d '{ "commands": [ { "cancel": { "index": "logstash-2017.07.26", "shard": 0}}]}'
 curl -XPOST http://localhost:9200/_cluster/reroute -d '{ "commands": [ { "cancel": { "index": "logstash-2017.07.26", "shard": 0, "node": "storj-prod-1"}}]}'
 curl -XGET http://localhost:9200/_cluster/allocation/explain | jq '.'
 curl -XGET http://localhost:9200/_cluster/reroute?retry_failed
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed
 curl -XGET http://localhost:9200/_cluster/allocation/explain | jq '.'
 ./list
 cd
 cd scripts/
 ./list_indices_sorted.sh
 curl http://localhost:9200/_cluster/health | jq '.'
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed -d '{ "commands": [ { "allocate_replica": { "index": "logstash-2017.07.26", "shard": 0, "node": "storj-prod-1"}}]}'
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed -d '{ "commands": [ { "move": { "index": "logstash-2017.07.26", "shard": 0, "from_node": "storj-prod-1", "to_node": "storj-prod-2"}}]}'
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed -d '{ "commands": [ { "move": { "index": "logstash-2017.07.26", "shard": 2, "from_node": "storj-prod-1", "to_node": "storj-prod-2"}}]}'
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed -d '{ "commands": [ { "move": { "index": "logstash-2017.07.26", "shard": 2, "from_node": "storj-prod-3", "to_node": "storj-prod-2"}}]}'
 ./list_indices_sorted.sh
 curl http://localhost:9200/_cluster/health | jq '.'
 curl -XGET http://localhost:9200/_cluster/allocation/explain | jq '.'
 curl -XPOST http://localhost:9200/_cluster/reroute?retry_failed -d '{ "commands": [ { "move": { "index": "logstash-2017.08.01", "shard": 0, "from_node": "storj-prod-1", "to_node": "storj-prod-2"}}]}'
 curl http://localhost:9200/_cluster/health | jq '.'
 curl -XGET http://localhost:9200/_cluster/allocation/explain | jq '.'
 curl http://localhost:9200/_cluster/health | jq '.'
 curl -XGET localhost:9200/_cat/shards?h=index,shard,prirep,state,unassigned.reason| grep UNASSIGNED
 curl http://localhost:9200/_cluster/health | jq '.'
 ./list_indices_sorted.sh
```

## Helper Scripts
  + [Shard Assignment Script](https://github.com/phutchins/elk-helpers/raw/master/scripts/assign_shard.sh)

## Helpful Links
  + [RED Elasticsearch Cluster? Panic no longer](https://www.elastic.co/blog/red-elasticsearch-cluster-panic-no-longer)
