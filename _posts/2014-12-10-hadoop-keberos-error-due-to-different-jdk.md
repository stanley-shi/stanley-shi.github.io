---
layout: post
title: hadoop Keberos Error Due To Different JDK
author: Stanley Shi
comments: True
tags: hadoop
---

## hadoop Keberos Error Due To Different JDK

I think this is one of the most stupid errors I have met in the Hadoop world. Let me explain.

### The sympoton

I was writing some Hadoop application and want to use Spring Batch to start the Hadoop job.

I finished the Hadoop application first and tried to use the "hadoop jar ***" command to submit the job. Everything works fine.

Then I wrote the Spring Batch code and configurations. In order to submit the Spring batch job using a simple CommandLineRunner, I wrote a simple bash script to collect all the commands and settings.

Now everything is done, I used the mavne assembly plugin to package the project as a tar ball, copy it to the test machine, run the shell...

"err..." error happens:

    2014-12-07 18:56:01 ERROR AbstractStep:225 - Encountered an error executing step t1mergestep in job t1mergejob
    java.io.IOException: Failed on local exception: java.io.IOException: javax.security.sasl.SaslException: GSS initiate failed [Caused by GSSException: No valid credentials provided (Mechanism level: Failed to find any Kerberos tgt)]; Host Details : local host is: "chd1b02c-0d79/10.98.50.84"; destination host is: "apollo-phx-nn.vip.ebay.com":8020;
    	at org.apache.hadoop.net.NetUtils.wrapException(NetUtils.java:764)
    	at org.apache.hadoop.ipc.Client.call(Client.java:1414)
    	at org.apache.hadoop.ipc.Client.call(Client.java:1363)
    	at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:206)
    	at $Proxy22.getListing(Unknown Source)
    	...
    	
But using the same shell (not other keberos related staff), I can successfully execute the commands like "hadoop fs -ls /";
This is strange, right?

### The cause

The cause is quite easy, and I will put it at the end. I will show you how I found it.

#### Trying to figure out
Trying to find the reason is really not a easy way. 

At first I thought maybe the Spring framework did something that masked the Keberos setup, so I wrote a simple java code to just use the HDFS API to get the content of a specified path. Using my bash script to start the program, it didn't work. This means **it's not the issue with Spring**.

Then I tried to use "hadoop jar" to run the same test java code, it works! This means there's some difference between the "hadoop jar" environment and my bash script environment.

Then I analyzed the "hadoop jar" related code until it reaches my own code, there's nothing special related with Keberos. So I changed my script to run the actual hadoop class (org.apache.hadoop.fs.FsShell) instead of my java program; The error is still there. This means that **it's not the problem with my java code**. The error exist outside of the java code.

Then I add "-x" to the hadoop command to print all the debug information; and also run my script using "sh -x myscript.sh *** "; By comparing the two outputs, and changing different params, I finally located the root cause. "hadoop jar" is using "/usr/java/latest/bin/java *** " to start the process but my script is using "/usr/bin/java" to start the process. Finally!!!

#### Root Cause
The system on which I ran the program has two JRE installed, one is the OpenJDK(1.6.0_20) and the other one is Oracle JDK (1.6.0_31).

In my shell scripts, I was using "which java" to detect the java runnable(which was "/usr/bin/java", pointing to OpenJDK); but in the Hadoop env, Oracle JDK is used (which was "/usr/java/latest/bin/java"). 

Then I changed my script to use the OracleJDK to run my program, then everything is fine.

I still don't know why this happens, but I don't have time for this now. Maybe I will come back to this sometime later.

