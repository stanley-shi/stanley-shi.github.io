---
layout: post
title: Running Couchbase in Docker
author: Stanley Shi
comments: True
tags: docker couchbase 

---

Very easy, use this command

    docker run  --detach -p 8091:8091 -p 8092:8092 -p 8093:8093 \
      -p 11207:11207 -p 11210:11210 -p 11211:11211 -p 18091:18091 \
      -p 18092:18092 --name cb-server --net=host couchbase:3.0.3

but don't miss the param "--net=host"; this param tells docker to use the hosts' network directly, it will listen on the hosts' ports directly;
Without this, if you want to access docker from other hosts, you will get into trouble.
    

### REF
http://developer.couchbase.com/documentation/server/4.1/install/docker-multiplehosts-singlecont.html
