---
layout: post
title: "Vim automatic wrapping at 80 characters"
description: ""
category: 
tags: [formatoptions 80 characters visual mode textwidth text formatting vi vim vimscript viml]
---
{% include JB/setup %}

If you want to force vim to always automatically wrap lines when it hits the 80
character mark, I would suggest doing the following:

First, edit your vim resource file:

    vim ~/.vimrc

Next, set the option for **textwidth** to the number of characters limit:

    set textwidth=80
    
Here's an alternative *shorthand version* of the textwidth option:

    set tw=80

Last, set the option for **formatoptions**:

    set formatoptions+=t

Here's an alternative *shorthand version* of the formatoptions option:

    set fo+=t

Enjoy,
Chip
