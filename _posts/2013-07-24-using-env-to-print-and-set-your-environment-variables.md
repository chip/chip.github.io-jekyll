---
layout: post
title: "Using env to print and set your environment variables"
description: ""
category: 
tags: [unix env pwd]
---
{% include JB/setup %}

From time to time, I need to know the value of a specific **UNIX** environment
variable.  Of course, that's easy enough to do if you know the exact spelling of
the variable, as in the case of the current working directory:

<br>
{% highlight bash %}

☺ echo $PWD
/Users/chip/code/chip.github.com

{% endhighlight %}

However, what if I'm unsure of the spelling, or I simply want to see all of the
variables that are set for my current environment?  That's easy enough to find
as well, using the `env` command (output below is limited for readability):

<br>
{% highlight bash %}

☺ env 
ARCHFLAGS=-arch x86_64
Apple_PubSub_Socket_Render=/tmp/launch-gANhjR/Render
Apple_Ubiquity_Message=/tmp/launch-yn5YIm/Apple_Ubiquity_Message
CLICOLOR=1
COMMAND_MODE=unix2003
DISPLAY=/tmp/launch-okscQD/org.macosforge.xquartz:0
EDITOR=/usr/bin/vim -f
.
.
.

{% endhighlight %}

That's pretty effective and since the environment variables are sorted, it makes
it easy to find a specific setting.

**But,** `env`, **can do more!**  It can also set the environment for you for specific
utilities.  Let's look at some example Ruby code to demonstrate this example:

<br>
{% highlight bash %}

☺ cat env.rb

{% endhighlight %}

<br>
{% highlight ruby %}

#!/usr/bin/env ruby

puts ENV['PWD']

{% endhighlight %}

Here you can see that this little script simply prints the contents of the `PWD`
environment variable to `STDOUT`.

For sanity, let's check the current setting of that variable just so we can see
if it matches our expectations:

<br>
{% highlight bash %}

☺ echo $PWD
/Users/chip/code/chip.github.com

{% endhighlight %}

That was what I expected.  So, let's say I want this script to use another
setting for that environment variable.  I'll try to set it first before calling
the script:

<br>
{% highlight bash %}

☺ env PWD=/tmp ./env.rb
/Users/chip/code/chip.github.com

{% endhighlight %}


**But, wait!  That didn't do anything.**  That's because the `env` command inherits
the current environment by default, which as we saw before had a different value
than the one we passed: `/tmp`.

The way to get around this is to pass the `-i` flag to `env`, like so:

<br>
{% highlight bash %}

☺ env -i PWD=/tmp ./env.rb
/tmp

{% endhighlight %}

Now it uses the variable as expected. **Keep in mind that it didn't set this
variable permanently**, but instead just for the execution of the utility we
passed it, which in this case was the `env.rb` script.

To prove it, let's take a look at the `PWD` variable one more time:

<br>
{% highlight bash %}

☺ echo $PWD
/Users/chip/code/chip.github.com

{% endhighlight %}

Based on the information above, if you ever need to review your current
environment, or set it for a single run of a utility, it's easy enough to do
using the `env` command.
