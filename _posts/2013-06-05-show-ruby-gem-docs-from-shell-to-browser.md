---
layout: post
title: "Show Ruby gem docs from shell to browser"
description: ""
category:
tags: [awk xargs unix grep gem ruby]
---
{% include JB/setup %}

# Background

Let's say I'm interested in learning more about a
particular Ruby gem, but I'd prefer to read the
documentation using a web browser.

## A starter example

I'll run this little command to find out more about pdf-reader:

    gem list pdf-reader -d

And here is the output...

    *** LOCAL GEMS ***

    pdf-reader (1.3.0, 1.2.0, 1.1.1)
    Author: James Healy
    Homepage: http://github.com/yob/pdf-reader
    Installed at
    (1.3.0): /Users/chip/.rvm/gems/ruby-1.9.3-p194
    (1.2.0): /Users/chip/.rvm/gems/ruby-1.9.3-p194
    (1.1.1): /Users/chip/.rvm/gems/ruby-1.9.3-p194

    A library for accessing the content of PDF files


## Full example

Since most gem authors include a Homepage for their documentation,
using this tiny mixture of Unix commands will do the trick (line
continuation character used only for formatting):

    gem list pdf-reader -d | grep Home | \
    awk '{ print $2 }' | xargs open

Enjoy,  
Chip
