---
layout: post
title: "Bash Tricks and Shortcuts"
date: 2015-04-12 09:03
comments: true
categories:
---

### Loops
Often times you need to run the same task in bash against a number of different arguments. Loops in bash can make this very quick and easy.

One of the simplest ways you can do this in a one liner is as follows
```bash
$ for i in one two three four; do echo $i; done
one
two
three
four
```

You can also predefine an array to use later like this
```bash
files=( "/tmp/file_one" "/tmp/file_two" "/tmp/file_three" )
for i in "${files[@]}"
do
  echo $i
done
```

Or, to do this on one line
```bash
$ files=("/tmp/file_one" "/tmp/file_two" "/tmp/file_three" ); for i in "${files[@]}"; do echo $i; done
/tmp/file_one
/tmp/file_two
/tmp/file_three
```

You can use ranges with `seq`
```bash
for year in $(seq 2000 2013); do echo $year; done
2000
2001
2002
2003
2004
2005
2006
2007
2008
2009
2010
2011
2012
2013
```

If you need a counter you could do something like this
```bash
#!/bin/bash
## declare an array variable
declare -a array=("one" "two" "three")

# get length of an array
arraylength=${#array[@]}

# use for loop read all values and indexes
for (( i=1; i<${arraylength}+1; i++ ));
do
  echo $i " / " ${arraylength} " : " ${array[$i-1]}
done
```
