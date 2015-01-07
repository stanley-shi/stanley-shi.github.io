---
layout: post
title: Simple interactive HDFS shell
author: Stanley Shi
comments: True
tags: utility hdfs

---

## Simple interactive HDFS shell
In some scenarios, you may need to do a lot of "hdfs dfs -***" commands and you will sure find it very boring to type the command again and agin.

Here's a simple script to save your effort. It is very easy and maybe error prone, but it DO save your time. 

So, instead of type "hds dfs -ls /user/stanley/", you can just type "ls /user/stanley" in the shell. This is a keyboard saver, right?

~~~
#!/bin/bash
HDFS_PROMPT='hdfs$ '
while :
do
  echo -n "$HDFS_PROMPT"
  read line
  if [ "quit" == "$line" ]; then
    break
  fi
  eval "hdfs dfs -$line"
done
exit 0
~~~

Note: 
1. On some system, the "echo -n" may not correctly, if you see ugly "-n" in the prompt, just remove the "-n" in the script;
2. You can modify it accordingly to provide a simple yarn application interactive shell.
