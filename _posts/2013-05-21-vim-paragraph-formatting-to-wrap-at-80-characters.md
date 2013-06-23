---
layout: post
title: "Vim paragraph formatting to wrap at 80 characters"
description: ""
category: 
tags: [80 characters visual mode textwidth text formatting vi vim vimscript viml]
---
{% include JB/setup %}

Forcing selected text to be limited to 80 characters within vim is fairly
straightforward.  

Just do this:

    1. Set the text width.
    2. Select desired lines.
    3. Reformat selected text.

Here's the breakdown in more detail.

From within vim, first set the text width setting so that vim knows how to
format the selected text:

    :set textwidth=80

Selecting the text can be done a number of ways, so for this example, let's say
my cursor was on the first line of a paragraph that I wanted to format.  The
cursor can be on any column of that line for this to work.  I would then do
this to select that paragraph:

    Shift-v }

Then I would reformat that paragraph, by just typing this:

     gq

Voila!  The text is then reformatted.

For details on how to keep these settings permanently, please check out my other
post on vim **textwidth**, [Vim automatic wrapping at 80
characters](/2013/02/14/vim-automatic-wrapping-at-80-characters/).

Enjoy, Chip
