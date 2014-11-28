---
layout: post
title: Tech Notes Continuously updated
author: Stanley Shi
Comments: True
tags: 
---

1. [SSH default settings](#how-to-setup-ssh-default-settings)
2. [Git alias](#how-to-enable-git-alias)
3. [Maven ssl error](#how-to-allow-maven-to-ignore-all-ssl-error)

-----
How to setup ssh default settings
=================================

Create a file at ~/.ssh/config with the following content:

    user stanley
    host mymac
        hostname mymac.corp.stanley.com
        

The above setting will set the default user name to all hosts as "stanley"; and then set the host alias "mymac" to the actual host "mymac.corp.stanley.com";

This alias is different from the /etc/hosts file that this alias will only affect the ssh command; if you ping "mymac", it will still warn you that this host is not known;

You can also set other setting here in the same file, please refer to [ssh config](http://linux.die.net/man/5/ssh_config) for more information;

-----
How to enable git alias
=======================

Create a file at ~/.gitconfig with the following content:

    [alias]
        co = checkout
        br = branch
        cm = commit
        pl = pull
        st = status

-----
How to allow maven to ignore all ssl error
==========================================

The original source is here: http://stackoverflow.com/questions/21252800/maven-trusting-all-certs-unlimited-java-policy

The solution is to add the following attributes to the maven command line: 

    -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true
  
