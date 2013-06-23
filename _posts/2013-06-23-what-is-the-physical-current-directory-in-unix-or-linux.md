---
layout: post
title: "What is the physical current directory in UNIX or Linux?"
description: ""
category: 
tags: [unix linux physical current directory pwd]
---
{% include JB/setup %}

So let's say I'm on my iMac and I change into the **/var** directory.

Guess what?  That's not the true or physical location on the file system.

Just try this and you'll be deceived:

    pwd

That command returns this:

    /var

But that doesn't resolve all of the symbolic links, so it's not the true
location.

To do that, pass the **-P** flag to the command:

    pwd -P

Which now returns the true location:

    /private/var

This is useful if you're ever deep in a directory structure that is symlinked,
you can always check your sanity and be sure of exactly where you are on the
system.

Enjoy, Chip
