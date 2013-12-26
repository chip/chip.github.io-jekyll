# Features of a UNIX shell

To get the most out of your system, you'll need to have a reasonable
understanding of the UNIX shell.  To that end, it's important to
understand that each shell has specific capabilities built-in including
commands such as `cd`, `pwd`, `kill` and many others, as well as certain
features that are specific to your current login session, which are
commonly referred to as your "environment".


## Environment variables

Within an "environment" a user has access to a number of variables that
helps them determine the current state of the session, as well as the
ability to manipulate that environment.

To have a clearer understanding of what an environment is, let's take
a look at the current state of it using the `env` command, which lists
all of the environment variables and their settings for the currently
logged in user:

    $ env
    TERM_PROGRAM=Apple_Terminal
    SHELL=/bin/bash
    TERM=xterm-256color
    TMPDIR=/var/folders/j9/mt6wbs411q3g6v4cmzczdm7m0000gq/T/
    Apple_PubSub_Socket_Render=/tmp/launch-lu8Krr/Render
    TERM_PROGRAM_VERSION=309
    TERM_SESSION_ID=ABE2DAAB-9168-414C-85B7-1EC9AC230A31
    USER=screencast
    COMMAND_MODE=unix2003
    SSH_AUTH_SOCK=/tmp/launch-swLXSs/Listeners
    Apple_Ubiquity_Message=/tmp/launch-XMri92/Apple_Ubiquity_Message
    __CF_USER_TEXT_ENCODING=0x1F7:0:0
    PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/go/bin
    PWD=/Users/screencast/Documents/learning-the-unix-command-line/text
    LANG=en_US.UTF-8
    HOME=/Users/screencast
    SHLVL=1
    LOGNAME=screencast
    DISPLAY=/tmp/launch-hjQmgV/org.macosforge.xquartz:0
    _=/usr/bin/env
    OLDPWD=/Users/screencast

Common environment variables include `SHELL`, `USER`, `HOME` and `PATH`.
Taking a look at each of these variables, we can see that we're running
the `bash` shell which is located under `/bin/bash`, our user is named
**screencast**, and the home directory for that user is located under
`/Users/screencast`, which is the standard expected location.


### The PATH environment variable

The last environment variable that I mentioned was the PATH, which is
extremely important.  Without it set properly, the shell would be
unusable because it determines the directories that should be searched
when typing in commands, as well as the order in which those directories
are searched.

As an example, let's take a look at the `ls` command.  As we did in
a previous chapter, let's take a look at the location of this command:

    $ which ls
    /bin/ls

As you can see, the location is `/bin/ls`, but normally we don't type
out the full path when running the command.  Doing so would be extremely
tedious, which shows just how important the PATH environment variable is
to using the shell in a convenient and effective manner.

So how does this work?  Well, for each command that is typed, the shell
will reference the setting of the PATH environment variable and attempt
to find the directory location of the command so it can be run properly.

Taking a look at our PATH variable, we see that it has the following
setting:

    PATH=/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/go/bin

Basically, a series of directories are listed, each of them colon
delimited.  So what the shell does is start by searching the `/usr/bin`
directory first, but for the `ls` command it doesn't find it there, so
it continues to the next directory listed, which is `/bin`.  Because the
`ls` command is located there, the shell stops from searching any
further and executes the command.

However, if we ran a command that wasn't installed under any of the
directories listed in our PATH, we'd get an error because the shell
couldn't find the command and run it.  Therefore, it's really important
that you understand what the PATH is used for and how to manipulate it
to suit your needs.

Let's say I have a small shell script that looks like the following:

    $ cat ~/bin/hello-world 
    #!/bin/sh

    echo "Hello World!"

As you can see, it's just a simple script that echoes "Hello World!" to
the screen, but in order to run it, I have to specify the full path:

    $ ~/bin/hello-world 
    Hello World!

What I'd like to do is to just type `hello-world`, just like I do with
other commands, but that currently won't work because the `~/bin`
directory located under my HOME directory is not located within my
current PATH.


### Setting environment variables

We can get this to work by changing my PATH, which can be done using the
`export` command:

    $ export PATH="$PATH:~/bin"

What I'm doing here is using the `export` command to specify the
variable to be changed, which in this case is the PATH, then use an
equals sign `=` to set it to a new value.  That value is a double-quoted
string which contains the current value of path, as indicated with
`$PATH`, followed by a colon to delimit the paths and then the directory
that I want appended.  

It's important to understand that the PATH variable is expanded within
the double-quoted string, so you end up with this: 

    $ echo $PATH
    /usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/opt/X11/bin:/usr/local/go/bin:~/bin

As you can see the `~/bin` directory is now located within our path, so
we can now run the script using the shorthand version:

    $ hello-world
    Hello World!

Bear in mind that the dollar-sign `$` is used to specify to the shell
that you want the value of the PATH variable and not the word PATH.


### Displaying environment variables

As you just saw with the `echo` command, you can inspect the current
value of an environment variable easily.  Let's take another look at
those common environment variables discussed earlier, this time viewing
them each individually using `echo`:

    $ echo $HOME
    /Users/screencast

    $ echo $SHELL
    /bin/bash

    $ echo $USER
    screencast


## Shell initialization files

Setting environment variables from within the shell is great when you
need to do a one-off or just need that variable set until you logout or
exit the session.  However, once you do that, those variables that you
manually exported will be lost since they aren't a permanent part of
your shell's environment.

To make environment variable changes persist to the next session, we'll
need to set them in an initialization file that gets run each time we
login.  Under the `bash` shell, this usually means setting environment
variables within either the `.bashrc` or `.bash_profile` files.


### Differences between .bashrc and .bash_profile 

On most UNIX systems running the bash shell, there are a number of files
that can be executed for each user, including `/etc/bashrc`,
`/etc/bash_profile`, `~/.bash_profile` or `~/.bashrc`, and the files
that are run depend on whether you're in running from a login or
non-login shell.  To complicate matters, the Mac's Terminal handles this
a little differently, which by default runs a login shell and in turn
executes `~/.bash_profile`.

Let me share a tip that will simplify your setup in such as way where it
works properly regardless of how you login.  First, create
a `~/.bash_profile` file with the following contents: 

    if [ -f ~/.bashrc ]; then
       source ~/.bashrc
    fi

What this does is look for a `~/.bashrc` file and if it exists, it
"sources" its contents, which means that it reads the file and executes
the commands within the shell.  Once this is done, all you need to do is
configure your system from this file, setting up your aliases, exporting
your environment variables such as your PATH, and many other things that
are specific to your account.

On another note, I would only edit the bash-related files that are
located under the `/etc` directory when I wanted the environment to be
changed for all accounts, as this is a system-wide change.


## Sourcing files

As we saw previously we can execute files by "sourcing" their contents
which is normally done with the `source` command.  As an alternative,
you can also use the dot `.` to handle sourcing.

In this example, I'm going to add an alias for changing into the
`~/Documents` directory, so let's first take a look to see how my bash
initialization files are setup:

    $ cat ~/.bash_profile 
    if [ -f ~/.bashrc ]; then
      source ~/.bashrc
    fi
    $ cat ~/.bashrc
    alias ".."="cd .."

We can see that my `~/.bash_profile` does nothing more than execute the
contents of my `~/.bashrc`, which itself contains only 1 alias for
navigating up a directory.

Let's now setup an alias called **docs** and append it to our
`~/.bashrc`:

    $ echo 'alias docs="cd ~/Documents"' >> ~/.bashrc

Let's then take a look at the file:

    $ cat ~/.bashrc
    alias ".."="cd .."
    alias docs="cd ~/Documents"

Now let's try it out:

    $ docs
    -bash: docs: command not found

As you can see, that didn't work, so what happened?  At this point the
shell doesn't have any knowledge of this alias since we only edited the
file, so let's make sure the alias is set by sourcing the file:

    $ . ~/.bashrc

As you can see, I used the shortcut approach to sourcing, but you could
have just as easily done this:

    $ source ~/.bashrc

Now let's try out the alias:

    $ docs

And verify which directory we're currently in:

    $ pwd
    /Users/screencast/Documents

Since we sourced it, the shell had the alias available and was able to
execute it.  Keep in mind that in addition to shell initialization
files, there are other applications on the system that use dot files,
such as vim, tmux, irb and many others, so anytime you make a change to
those files, you'll need to source them before those changes take
effect.


## Shebang #!

In UNIX you'll often hear users refer to the first line in a script as
the "shebang" line, which is represented as a hash mark `#` (or pound
symbol) followed by an exclamation point `!`, and then the path to the
language interpreter.

Here are some common examples of shebang lines in UNIX:

1. Bash scripts

  #!/bin/sh

2. Perl scripts

  #!/usr/bin/perl

3. Ruby scripts

  #!/usr/bin/ruby

What this does is to instruct the shell to execute the contents of the
file using the interpreter located on the first line.


## Repeating commands

Let's say that I want to repeat one of the last commands that I wrote,
but I don't want to have to type it out again because that is too
time-consuming.

One approach I could use is to simply scroll through my command history
using the up and down arrows on my keyboard.  That works ok, I guess,
but there are much better alternatives.


### Repeat the last specific command

One easy way is to use the "bang operator", which is represented using
an exclamation point `!`.  As an example, let's say I wanted to run the
last `echo` command.  I could do that by typing `!echo` ("bang" echo):

    $ !echo
    echo $USER
    screencast

This runs the `echo` command that I last typed, which was for the USER
environment variable.  First it prints the command to the screen, runs
that command and then displays the output of the command as usual. 


### Repeat the last command

If I wanted to repeat the very last command I typed, I don't have to
specify the command used, but instead just type `!!` ("bang bang"):

    $ !!
    echo $USER
    screencast


### Bang command:p

Since using the bang command could potentially be dangerous, there's
a safe way to specify the command first without running it, which is to
append `:p` to the bang command you're using:

    $ !!:p
    echo $USER

What this does is simply print the last command to your screen, but it
doesn't run the command.  If you feel like the output is safe to run,
then you can just use the "bang-bang" version to actually run the
command.


### History

The approaches already described can be quite helpful in some cases, but
a lot of times you want to repeat a command that you typed in long ago.
To add to the complexity, the command might involve a long series of
flags or options and although you might remember the command you used,
you might not remember it exactly.

Here's where running your history comes in handy:

    $ history
    628  echo $USER
    629  pwd
    630  grep -i history *
    631  vi 07_redirection_and_pipes.md 
    .
    .
    .

On the left, you have the command number that history has stored, which
you can use to repeat the command.  For example, let's say I wanted to
run the `grep` command shown here.  I could simply type bang 630 to
repeat it:

    $ !630
    grep -i history *
    01_why_should_i_learn_the_unix_command_line.md:# Brief history
    04_getting_help.md:**ENVIRONMENT**, **HISTORY**, **BUGS**, **CONFORMING
    .
    .
    .

What this does is print the command identified by history as 630, and
then run that command.


### Ctrl-R

For repeating commands, the most useful approach that I've found so far
is to use `Ctrl-R`, which does a reverse search through your shell's
history.  Let's try it out:

    $
    (reverse-i-search)`echo': echo $PATH

Typing the keystroke combination of the `Ctrl` key followed by the
letter `r`, will activate the reverse search, which is case insensitive.
As I type in the letters for `echo`, it attempts to find a match upon
each keystroke.  I can also scroll through those matches by continuing
to press `Ctrl-R`.  Once I've found the command I want to execute, I can
simply press the Enter key.

If you pass up a command when using `Ctrl-R` an want to go back, simply
type `Ctrl-S`.  

In my shell, this didn't work out of the gate, so I had to do the following:

    stty -ixon

This sets the terminal, which according to the man pages, enables or
disables the START/STOP output control.  

Of course, the `Ctrl-R` technique only works based on the amount of
history your shell has saved.  Since the default size for your history
is 500 commands, you might want to increase that, so just be sure to
export the HISTSIZE and HISTFILESIZE variables from your `~/.bashrc`.
The HISTSIZE is used for the number of commands that should be
remembered in your history, while the HISTFILESIZE dictates the limit of
commands that should be saved to the history file.


## Run command sequentially

Occasionally you'll need to run a series of commands together, but you'd
rather not type them out individually.  Remember that the bash shell
allows you to separate commands using a semi-colon, so instead of doing
something like this:

    $ pwd
    /Users/screencast

    $ cd ~/Documents

    $ ls -l
    total 0
    drwxr-xr-x  11 screencast  staff  374 Sep  7 09:37
    learning-the-unix-command-line
    drwxr-xr-x  14 screencast  staff  476 Sep  7 09:33 screencast-osx

You could instead type all of the commands on the same line, separating
them using a semi-colon:

    $ pwd; cd ~/Documents/; ls -l
    /Users/screencast
    total 0
    drwxr-xr-x  11 screencast  staff  374 Sep  7 09:37
    learning-the-unix-command-line
    drwxr-xr-x  14 screencast  staff  476 Sep  7 09:33 screencast-osx


## Command line navigation:

To wrap up this chapter, I'd like to share with you a few tips on
navigating the cursor from the command line.  Here are some shortcuts
that I use quite a bit in my everyday work:

    Ctrl-l - Clear the screen
    Ctrl-u - Clear the command line
    Ctrl-a - Go to the beginning of the shell prompt
    Ctrl-e - Go to the end of the line

This is a lot easier than using the arrow keys and allows you to
continue your work without interrupting your rhythm.


## Summary

So that wraps up this chapter on UNIX shell features.  We've looked at
environment variables, shell initialization files and a number of other
tricks that can make your work flow more streamlined.  Hopefully you can
take what you've learned here and continue to improve your knowledge of
the shell, improving the configuration of your system as you learn more.
