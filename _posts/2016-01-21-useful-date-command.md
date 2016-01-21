---
layout: post
title: Useful 'date' command -- find two days before some day
author: Stanley Shi
comments: True
tags: shell 

---

## Useful 'date' command -- find two days before some day
I have a requirement to find the date of some day that is 2 days before some specified date.
Searched through the internet, finally find the solution:

    $ date -d "2016-01-01 2 days ago" +%Y-%m-%d
    
This command will return "2015-12-30".

And similarly, if you want to find two days after some day, it will be:

    $ date -d "2016-01-01 2 days ago" +%Y-%m-%d
    
This command will return "2016-01-03"


### REF
http://superuser.com/questions/45592/how-to-add-a-day-to-date-in-bash
