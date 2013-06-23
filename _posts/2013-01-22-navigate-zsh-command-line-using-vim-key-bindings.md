---
layout: post
title: "Navigate the zsh command line using vim key bindings"
description: ""
category: 
tags: [bindkey bindings key vim zsh zshrc navigate]
---
{% include JB/setup %}

If you use zsh, you can easily edit the command line using vim key bindings.
Just add this to your `~/.zshrc`, like so:

    bindkey -v

Be sure to "source" the shell initialization afterwards with:

    source ~/.zshrc

Here's another shortcut for accomplishing the same sourcing:

    . ~/.zshrc

Now you can navigate the command line using vim key bindings.  This has been a
huge time-saver for me, so give it a try.

Enjoy,
Chip
