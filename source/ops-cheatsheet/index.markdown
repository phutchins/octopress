---
layout: page
title: "DevOps (Operations) Cheatsheet"
date: 2015-06-04 10:23
comments: true
sharing: true
footer: true
categories: [cheatsheet, linux]
---

## Determine open files (file descriptors) for all processes by a user
```bash
watch 'lsof -u bws | awk '\''{print $2}'\'' | sort | uniq -c | sort -n'
```

## Check iowait and disk stats
Use sar (which you will have had to install and enable previously) or...

```bash
iostat -xm 5
```

## Create CSV from data in MongoDB
Please note that this requires jq 1.5 or greater for the join feature

First, create a js file "myquery.js" containing the query you would like to run
```
use mydatabase
db.getMongo().setSlaveOk()
printjson( db.users.find({}, { _id: 1, hashpass: 1 }).toArray() )
```
This sets the database to `mydatabase`, lets us read from a slave in a replicaset (only do
    this if you are using a replicaset), then finds all records in the users collection, only
selecting the `_id` and converting it to an array

To execute this script against your DB from the command line, you would then do something like
```
mongo < myquery.js > user_list.json
```

First, remove the extra data thats been added before and after the []'s in the output file so
it is valid json

Then parse the json data and convert it to a CSV
```
cat user_list.json | jq -r '. | map([._id, .hashpass] | join(", ")) | join("\n")' > user_emails_hashpass.csv
```

The last step is to add a header to your CSV file
```
sed -i "Email Address, Hashpass" user_emails_hashpass.csv


## LVM Notes

To import a Logical Volume that was not exported before being removed from a host, you can use

`vgchange -ay`

to import the LV again. You will likely have to run `pvscan` and `vgscan` before doing this.
