---
layout: post
title: Tech Notes Continuously updated
author: Stanley Shi
Comments: True
tags: 

---
[Maven ssl error](#how-to-allow-maven-to-ignore-all-ssl-error)


How to allow maven to ignore all ssl error
==========================================

The original source is here: http://stackoverflow.com/questions/21252800/maven-trusting-all-certs-unlimited-java-policy

The solution is to add the following attributes to the maven command line: 

    -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true
  
