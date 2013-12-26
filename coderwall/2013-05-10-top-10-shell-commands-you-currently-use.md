#Top 10 shell commands you currently use

First off, **I need to credit [Ben Orenstein](https://twitter.com/r00k) for
this idea**.  Since I've found it to be very useful, I thought others could
benefit from it, so here goes...

From time to time, I like to analyze which **UNIX** or **Linux** shell commands I'm
using most frequently.  To do this, all I need is a little `awk`, like the
following:

    history | \
    awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | \
    sort -rn | \
    head

So what does this do?  

Basically, it parses your shell command history looking at the second column, which is the
command you typed, and then increments a counter each time it is found.

Then it displays a sorted report showing the count of the command and the
command itself *without arguments*, like this example on my iMac:

    1705 git
    1420 ack
    1016 vi
    501 ls
    490 commit
    310 cd
    211 cat
    202 g
    191 rm
    181 c

Based on this output of the top 10 most frequently used shell commands, it
shows that I'm using git, ack, and vi a TON, so it would be helpful to create
aliases for these commands.  I generally prefer 1-character aliases if I can
get away with it.  Here are a few examples:

    alias a="ack"
    alias g="git"
    alias v="vi"
    alias l="ls -al"
    alias c="git commit -m"

I have found that these aliases alone have sped up my workflow significantly,
as well as reduce the wear and tear on my fingers.

This has become so useful that I created an alias for it as well, which I'll
cover in a later tip.

Please try it out and let me know your thoughts.
