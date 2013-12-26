# Create a zsh function

In a [previous tip](http://chip.github.io/2013/05/10/top-10-shell-commands-you-currently-use/), I discussed how I analyze my shell history to determine
which commands I use the most, just so I can then create short aliases for
those commands to minimize keystrokes.

That command itself used a little awk and I always forget exactly how it works,
so I decided to create an alias for it.  Unfortunately, that alias was so long
and difficult to read that I decided to refactor part of it into a **shell
function**.

Since I use **zsh** on my systems, I needed to edit **~/.zshrc** and add the following
function:

    function commands() {
        awk '{a[$2]++}END{for(i in a){print a[i] " " i}}'
    }

Then I create the alias I need to display the top 10 most frequently used shell
commands that I type, which are as follows:

    alias topten="history | commands | sort -rn | head"

First, it shows the **history**, pipes the output to the **commands** function I
created, then numerically sorts the output in reverse order, and displays only
the first 10 results using **head**.

Now you can create zsh functions *and* improve your workflow by analyzing
your heavily used commands.  

