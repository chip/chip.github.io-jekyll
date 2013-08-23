---
layout: post
title: "Do you manage a lot of servers with ssh?"
description: ""
category: 
tags: [hostname ssh]
---
{% include JB/setup %}

If you have a lot of servers that you manage and you can't remember all of the
IP addresses, add a configuration like this to `~/.ssh/config`:

<br>
{% highlight bash %}

Host macbook
HostName 192.168.1.50
User chip

{% endhighlight %}

Substitute the values for **Host**, **HostName** and **User** to suit your specific
needs.

Logging in is now as easy as this:

<br>
{% highlight bash %}

ssh macbook

{% endhighlight %}
