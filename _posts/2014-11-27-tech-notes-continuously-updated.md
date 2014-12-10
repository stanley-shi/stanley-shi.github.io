---
layout: post
title: Tech Notes Continuously updated
author: Stanley Shi
comments: True
tags: 
---

+ [Remove windows newline in vim](#remove-windows-newline-in-vim)
+ [Minux("-") in path](#minus-sign-in-path)
+ [Hadoop job submission](#hadoop-job-submission-notes)
+ [SSH default settings](#how-to-setup-ssh-default-settings)
+ [Git alias](#how-to-enable-git-alias)
+ [Maven ssl error](#how-to-allow-maven-to-ignore-all-ssl-error)

Remove windows newline in vim
=============================
Or say, remove the "^M" character at the end of lines in vim, the original post is here: http://www.tech-recipes.com/rx/150/remove-m-characters-at-end-of-lines-in-vi/

To remove the ^M characters at the end of all lines in vi, use:

    :%s/^V^M//g

The ^v is a CONTROL-V character and ^m is a CONTROL-M. When you type this, it will look like this:

    :%s/^M//g

In UNIX, you can escape a control character by preceeding it with a CONTROL-V. 

Minus sign in path
==================

If a linux folder has minus ("-") sign as the first character, here's how to delete it:
    $ rm -- -2345


Hadoop job submission notes
===========================

The original post is here: http://grepalex.com/2013/02/25/hadoop-libjars/
Main point:

1. add libjars to the options:

            $ export LIBJARS=/path/jar1,/path/jar2
            $ hadoop jar my-example.jar com.example.MyTool -libjars ${LIBJARS} -mytoolopt value

2. Make sure your code is using GenericOptionsParser

    public static void main(final String[] args) throws Exception {
        Configuration conf = new Configuration();
        int res = ToolRunner.run(conf, new com.example.MyTool(), args);
        System.exit(res);
     }
     public class SmallFilesMapReduce extends Configured implements Tool {
        public final int run(final String[] args) throws Exception {
            Job job = new Job(super.getConf());
            ...
            job.waitForCompletion(true);
            return ...;
        }
    }
          

3. Use HADOOP_CLASSPATH to make your third-party JAR’s available on the client-side

        $ export LIBJARS=/path/jar1,/path/jar2
        $ export HADOOP_CLASSPATH=/path/jar1:/path/jar2
        $ hadoop jar my-example.jar com.example.MyTool -libjars ${LIBJARS} -mytoolopt value

How to setup ssh default settings
=================================

Create a file at ~/.ssh/config with the following content:

    user stanley
    host mymac
        hostname mymac.corp.stanley.com
        

The above setting will set the default user name to all hosts as "stanley"; and then set the host alias "mymac" to the actual host "mymac.corp.stanley.com";

This alias is different from the /etc/hosts file that this alias will only affect the ssh command; if you ping "mymac", it will still warn you that this host is not known;

You can also set other setting here in the same file, please refer to [ssh config](http://linux.die.net/man/5/ssh_config) for more information;

How to enable git alias
=======================

Create a file at ~/.gitconfig with the following content:

    [alias]
        co = checkout
        br = branch
        cm = commit
        pl = pull
        st = status


How to allow maven to ignore all ssl error
==========================================

The original source is here: http://stackoverflow.com/questions/21252800/maven-trusting-all-certs-unlimited-java-policy

The solution is to add the following attributes to the maven command line: 

    -Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true
  
