---
layout: post
title: "Copy and paste from the command line on OS X"
description: ""
category: 
tags: [copy paste osx pbcopy pbpaste clipboard pasteboard mac pipe rspec spec]
---
{% include JB/setup %}

Most Mac users know they can **copy** and **paste** to the clipboard either by
using this feature that is already provided by the application's *Edit* menu,
or by using the counterpart **Command-c** and **Command-v** keyboard shortcuts.
But do they know that OS X also has commands that allow you to **copy** and
**paste** directly from the command line?

More often than not, I'm surprised by my pair programming partners who are
unaware of these very useful commands, so I felt like it was a good opportunity
to demonstrate how to use them.

Let's say I have some failing tests that I want to share with someone, in hopes
that they can help me get unblocked on a task.  There are many ways to share
the results, but I find that with my co-workers we often post a **Gist** to
[https://gist.github.com/](https://gist.github.com/) so that we can review and
comment on it as a team.  I'm usually working directly from the command line
using the **Terminal** application, and would prefer to do what is necessary
directly from there as opposed to using my trackpad to manually select the test
results.

"So what do you use to capture the data?", you ask?

Well, I usually reach for **pbcopy**.

Then I can just pipe the results of running the spec, as shown here:

    rspec spec/models/whatevs.rb | pbcopy
    
This simply runs **Rspec** and sends it's `STDOUT` (i.e., *standard output*) to
**pbcopy**, which copies that data to the clipboard (i.e., pasteboard).

Now I can just head over to my web browser and paste in the output at
[https://gist.github.com](https://gist.github.com) by using the **Command-v**
keystroke shortcut (or by using *Edit* -> *Paste* from the browser if you're so
inclined).

Now what if I decide that I would rather email this output to a co-worker as an
attached file?

Since the data is currently stored on the clipboard, I can use counterpart to
**pbcopy**, which as you might have guessed, is **pbpaste**.

Let's see that in action:

    pbpaste > ~/Desktop/whatevs-results.txt
    
This command takes the results stored on the clipboard and redirects it to a
newly created file under my **Desktop** folder.

These commands also have additional options for specifying *which* pasteboard
to use, as well as what *data type* to look for, but I won't be covering those
details here since I personally haven't used them yet.  However, if you're
interested in the nitty-gritty, please see `man pbcopy` for more details.

So, next time you need to copy and paste from the command line, keep in mind
that you might not need to use your mouse or trackpad, but instead can use a
combination of **pbcopy** and **pbpaste** to get the job done!
