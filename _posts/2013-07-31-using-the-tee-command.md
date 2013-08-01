---
layout: post
title: "Using the tee command"
description: ""
category: 
tags: [unix linux tee stdout df]
---
{% include JB/setup %}

Hey there!  

It's time for another installment of UNIX command line tips.  In the
previous newsletter, we talked about the `env` command and how you can
use it to view your account's environment variables, as well as a couple
of other neat tricks.  This week, we're going to demonstrate something
completely different.  I hope you enjoy it.

Let's say you're interested in how much free disk space you have on your
system.  

That easy enough:

    iMac:~ $ df -h
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  151Gi   82Gi    65% 39657424  21412016   65%  /
    devfs          204Ki  204Ki    0Bi   100%      704         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

But what if we also want to save those results to a file?

That would be easy to do as well, just by redirecting standard output
(i.e., stdout) to a file:

    iMac:~ $ df -h > df.out

**That works fine, except that I don't want to type 2 separate commands.**

In fact, for some examples, it might not be feasible to run the commands
separately because the output might differ.  What you need instead is
the ability to capture the output, send it to standard output (e.g., the
terminal), and also save it to a separate file.  

Well, there's a command that will help you do just that - the tee command.  

**According to the UNIX man pages on OS X:**
*The tee utility copies from standard input to standard output, making
a copy in zero or more files.*

Let's try our example again, but this time using tee:

    iMac:~ $ df -h | tee df.out
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  151Gi   82Gi    65% 39657219  21412221   65%  /
    devfs          204Ki  204Ki    0Bi   100%      704         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

Now let's take a look at the contents of that file, just to be sure that
the disk information was written there in addition to what we saw on
standard output:

    iMac:~ $ cat df.out 
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  151Gi   82Gi    65% 39657219  21412221   65%  /
    devfs          204Ki  204Ki    0Bi   100%      704         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

The output is identical to what we saw when we ran the original command,
which is exactly the result we want.

So, next time you need to see standard output and also copy it to
a file, remember that the tee command is a great option.

