---
layout: post
title: "Remove remote git branches merged into master"
description: ""
category: 
tags: [git sed grep xargs]
---
{% include JB/setup %}

Ok, let's say that you've been working a shit-ton on pushing various topic 
branches using git.  Yeah, it happens.  And later in the day you then move to 
another machine where you don't have those branches stored locally.

It's time to do a little **git housekeeping**.

So, let's get the latest updates:

    git pull --rebase origin master
    git fetch

I normally prefix all of my branches with my name, like `chip/tricked_out_feature`,
just so my branches are all grouped together. At work, I also prepend the 
branch with an id from Pivotal Tracker since that's how we manage the project 
*(as you can see in the example below)*.

This makes finding my branches, easy...

    git branch -r | grep chip

...which will normally return results that look like this:

    origin/chip/41096601_remove_cc_billing_address_id
    origin/chip/48334463_duplicate_extensions

Now, in this case, I know both branches have been merged, so it's easy to 
remove them one at a time.  However, sometimes I have so many branches and 
I'm not sure if certain ones have been merged already.

This command will help me clean up my mess quite nicely *(and avoid trashing*
`master` *in the process)*:

    git branch -r --merged master | \
    sed 's/ *origin\///' | \
    grep -v 'master$' | \
    xargs -I% git push origin :%

This has helped me quickly do housekeeping on branches that I no longer need, so 
it's much easier to glance at my list of active branches.

I've always found that the less clutter I have around, whether it's on my Desktop 
or my git repos, the clearer I think because it doesn't distract me with 
useless garbage.

Hopefully this will work for you as well.

