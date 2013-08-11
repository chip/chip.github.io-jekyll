---
layout: post
title: "Using curly brace expansion in bash"
description: ""
category: 
tags: []
---
{% include JB/setup %}

# Using curly brace expansion in bash

Did you know that UNIX's bash shell offers a shortcut for specifying
sometimes complex directory structures and filename combinations?

Well, it can, and that functionality is generally referred to as "curly
brace expansion", or just simply "brace expansion".  To illustrate this
further, let's walk through a few examples of it's usage.

First, let's say you wanted to create the following files using the
`touch` command.  Here's the longhand version of how you could
accomplish that:

    chip@iMac ~/code/foo
    ☺ touch 1.txt 2.txt 3.txt 1.out 2.out 3.out
    chip@iMac ~/code/foo
    ☺ ls -l
    total 0
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 1.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 1.txt
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 2.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 2.txt
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 3.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:45 3.txt
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 config
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 db
    drwxr-xr-x  5 chip  staff  170 Aug 11 13:13 public
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 views

That works fine, but if you had to create a lot of files it might become
far too cumbersome.

There's an easier way.

Let's try it again, but this time using the brace expansion technique:

    chip@iMac ~/code/foo
    ☺ touch {1,2,3}{.out,.txt}

And here are the files that the command created:

    chip@iMac ~/code/foo
    ☺ ls -l
    total 0
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 1.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 1.txt
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 2.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 2.txt
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 3.out
    -rw-r--r--  1 chip  staff    0 Aug 11 13:38 3.txt
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 config
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 db
    drwxr-xr-x  5 chip  staff  170 Aug 11 13:13 public
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 views

This produces the same result as before, but in a much easier fashion.

We can also use brace expansion for removing files as well, so let's
clean up our mess:

    chip@iMac ~/code/foo
    ☺ rm {1,2,3}\*

And again, looking at the contents of our directory:

    chip@iMac ~/code/foo
    ☺ ls -l
    total 0
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 config
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 db
    drwxr-xr-x  5 chip  staff  170 Aug 11 13:13 public
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 views

As you can see, those files have now been removed.

Another usage of this technique can be applied when creating
directories.  Here's what I used recently for
a [Sinatra](http://sinatrarb.com) project that I was working on
recently:

    chip@iMac ~/code/foo
    ☺ mkdir -p config public/{images,javascripts,stylesheets} views db

And let's take a look at the result:

    chip@iMac ~/code/foo
    ☺ ls -lR
    total 0
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 config
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 db
    drwxr-xr-x  5 chip  staff  170 Aug 11 13:13 public
    drwxr-xr-x  2 chip  staff   68 Aug 11 13:13 views

    ./config:

    ./db:

    ./public:
    total 0
    drwxr-xr-x  2 chip  staff  68 Aug 11 13:13 images
    drwxr-xr-x  2 chip  staff  68 Aug 11 13:13 javascripts
    drwxr-xr-x  2 chip  staff  68 Aug 11 13:13 stylesheets

    ./public/images:

    ./public/javascripts:

    ./public/stylesheets:

    ./views:

So that's the basic idea of the brace expansion technique in bash.
I encourage you to think about other combinations that might come in
handy in your own work flow.

