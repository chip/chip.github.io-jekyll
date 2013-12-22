---
layout: post
title: "The bang operator"
description: ""
category: 
tags: []
---
{% include JB/setup %}

As mentioned back in July with the newsletter, [Top 10 shell commands
you currently
use](http://us7.campaign-archive2.com/?u=fd1f24aadaae48b22d7d03a3b&id=fd7c6dc6c1&e=[UNIQID]),
I showed how the `history` command displays the command history list
with line numbers.  Here is a sample of the last 5 lines of `history` on
my local system:  
                                     
    1786  rake post title="The-bang-operator"
    1787  vi \_posts/2013-12-22-the-bang-operator.md
    1788  man history
    1789  help history
    1790  fg

We can make use of our command history more effectively by using the
shell "bang operator".
<br />
<br />

## shell ! (aka bang operator)

The line numbers shown in history output are used as command
identifiers.  Therefore, if I wanted to run the `rake` command shown
above, all I would need to do is specify the bang operator provided by
the shell, followed immediately by the number associated with the
command:

    !1786

Alternatively, I could also use the bang operator before a command:

    !rake

This allows you to run previous commands quickly without having to
remember all of the options, switches or arguments that were passed to
the command.
<br />
<br />

## Using :p with the bang operator

However, what if the command you want is far back in your history and
you do not want to run history and scroll through a heavy amount of
output?  One solution is to use `:p` with the bang operator, which
allows you to recall that last command, but without running it:

    !rake:p
    rake post title="The-bang-operator"

This is useful because sometimes you have other commands that might
match the command pattern you specify with the bang operator, yet those
commands might be unsafe to run.  In other words, what if the last
`rake` command I ran did something destructive?  By using `:p`, it
allows you to recall the command safely and then decide if it is safe to
run.
<br />                                                         
<br />                                                         

## "Learning the UNIX command line on OS X" screencast

Since my last newsletter, I have produced a screencast called,
["Learning the UNIX command line on OS
X"](https://www.udemy.com/learning-the-unix-command-line-on-os-x/?couponCode=HOLIDAYNEWS2013),
which contains **over 50 lectures and 3.5 hours of content**.

[![Learning the UNIX command line on OS
X](/assets/images/udemy-screencast.png)](https://www.udemy.com/learning-the-unix-command-line-on-os-x/?couponCode=HOLIDAYNEWS2013)

**As appreciation for my newsletter subscribers, I am offering a coupon
for purchasing the screencast at $35, which is 64% off the current $99
Udemy price.  This offer is only available through December 25, 2013.**

Be sure to use coupon code **HOLIDAYNEWS2013**, which is already
included in the link above.

Happy Holidays!

Chip Castle
