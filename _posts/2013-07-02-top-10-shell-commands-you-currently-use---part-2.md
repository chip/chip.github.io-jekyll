---
layout: post
title: "Top 10 shell commands you currently use   Part 2"
description: ""
category: 
tags: [shell linux history unix commands head sort uniq cut]
---
{% include JB/setup %}


In a [previous post](/2013/05/10/top-10-shell-commands-you-currently-use/), I
demonstrated how you can get a list of the **Top 10 shell commands** you
currently use, and how you can use this information to improve your workflow.

Since then I've received a number of suggestions and have also thought about
this command a good bit more, so I thought that I could offer a variation on
the implementation.

The previous version used **awk**, but this one will instead use **cut**, as
follows:

    history \|
    cut -c8- \|
    cut -d' ' -f1 \|
    sort \|
    uniq -c \|
    sort -nr \|
    head

I've formatted this solution to show only one command per line, just so that
it's easy to read and follow my step-by-step description.

**1.**  To start off, and without surprise, we issue the **history** command.

**2.**  Next, we use **cut** to remove the first 8 characters only of the
output.  **My system is OS X, so just keep in mind that if you're on another
system, the spacing of the output might possibly vary a little.**

**3.**  Again we use **cut**, but this time we accept the output of the
previous command by using the **-d** option, where we specify it to split on a
single space, but ask for only the first field of the result set to be returned
using the **-f** option.

**4.**  Next up is really nothing special.  We simply **sort** the output.

**5.**  We then filter the sorted results using **uniq**, but we've made sure
to include the **-c** option to precede each output line with the count of the
number of times the line occurred in the input, followed by a single space.
Special thanks to @tdl for this tip in my [previous
post](https://coderwall.com/p/o5qijw).

**6.**  To round out the most frequently used commands, we are sure to **sort**
the output numerically due to the command counts provided by **uniq**, and to
be sure to do so in reverse order since we're interested in the most frequently
occurring commands first.

**7.**  Lastly, we use **head** to show the **Top 10 shell commands**.  Of
course, you could always vary this a bit if you're interested in more than 10
by simply specifying **-n count**, where **count** should be replaced with the
number of results you desire.

The previous tip was so popular that I felt this merited some more thought.  I
think it's a great example not only of how to improve your workflow, but also
how shell commands can generally solve problems in variety of ways.

Enjoy, Chip
