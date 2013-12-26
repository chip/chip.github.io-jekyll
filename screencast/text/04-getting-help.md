# Getting Help

In this chapter, we'll be covering various ways of getting help on a UNIX system.

What's the best way to accomplish this?  For starters, you could always
do a Google search for specific help, but sometimes an Internet
connection isn't available, so you need an alternative way of looking up
information.

In my opinion, the most straightforward way to find help is to just use
the UNIX manual that comes installed on your system.  Most UNIX users
refer to that manual simply as "the man pages".  


## Man page structure

Taking a look at the structure of a man page, you'll often find that
there is a **NAME** heading, which is used to describe a command or
system function, followed by a **SYNOPSIS** heading, which describes the
basic usage of that command.  After that, you'll most likely see
a **DESCRIPTION**, which should provide an expanded explanation of the
command, followed by a heading for **OPTIONS**, which lists the various
flags or switches that a command supports.

A UNIX man page will often refer to many other items.  Here's a brief
list of some of the more common ones that you'll find:

    ENVIRONMENT
    HISTORY
    BUGS
    CONFORMING TO
    AUTHOR
    COPYRIGHT
    COMPATIBILITY
    EXAMPLES
    DIAGNOSTICS
    SEE ALSO

Please take note of the **SEE ALSO** heading of any man page, as it
provides references to related commands that you might find helpful.

Remember that there is not a fixed structure to these documents.
Software engineers from around the world contribute to these pages, and
each one generally has their own preferred format. One man page can
certainly differ from another, but you'll often see many of these
headings as a general convention, so it's helpful to be aware of them.


## Man page style

`man` pages are not meant to cover every nook and cranny of a given
command, but instead should be used as a general guide for its usage.
This can be a stumbling block to UNIX users, especially those who are
beginners.  Just keep in mind that as you become more comfortable and
proficient with the operating system, you'll find the `man` pages to be
an extremely valuable resource, so it's useful to get accustomed to
using `man` pages in the early stages of your learning process, so that
you can develop good habits for the future.


## Man page syntax

Let's dive into the syntax of a man page.  For this example, I'm going
to select another command.  From the Terminal, type in `man` followed by
`ls`:

    man ls 

What this says is, "please give me the man page for the ls command".
Looking at the man page for `ls` we can see a **NAME** heading, which
describes how this command is used to "list the directory contents".  If
you're not familiar with directories, just think of them as a folder or
container on your system which can contain files,and other directories.  

From the **SYNOPSIS** we can see that the `ls` command has
quite a large number of flags that are listed in bold.  All of these flags are
surrounded by brackets, so that tells us that they're all completely optional.
After the flags we see another pair of brackets with the word "file" underlined.
The use of the underline means that this file is a user-specified argument.
But since this file argument is displayed within brackets, we know that it is
also optional.  

Let's see this in action.  From my current directory, I can type in
the `ls` command and see its output.  Since I didn't specify any
arguments, it will list the contents of the current directory by
default.  The detail is sparse, so let's try to find out more.
Revisiting the man page for the `ls` command, we can review a list of
available options.  One popular option is a flag for listing the "long"
format of a directory contents, which is demonstrated by issuing the
following:

    ls -l

As you can see, the result of that command shows the contents of the
directory, but the output is much different than before.  This time it
shows the permissions of the directory or file, the amount of links or
directories found within that directory, the user and group ownership of
the file, the file size, the date of last modification of the file, and
finally the name of the file or directory.


### User specific arguments

Looking at the man page one last time we see the user-specified
file argument is followed by an ellipsis, which indicates additional
arguments can be specified.  Revisiting our example, I could demonstrate
passing multiple arguments to the `ls` command by running the following:

    ls -l Desktop Documents Music

The result now only shows the contents of those directories I specified
as optional arguments, as opposed to showing all of the contents of the
current directory as it did in the first command.


## Mutually exclusive options

Some `man` pages will specify options or flags separated by a vertical
bar or pipe.  Since the `ls` command doesn't have this in its `man`
page, I'll need to show you another command to demonstrate this syntax.
From the Terminal, type in the man page for the `pwd` command:

    man pwd 

The **SYNOPSIS** of this man page shows that the `pwd` command
can accept 2 different options, as indicated by the brackets, and that
these options are mutually exclusive, as indicated by the vertical bar.
In other words, the `pwd` command can accept either an `-L` option or
a `-P` option, but using them both together might not have the effect
that you expect.

Following the man page, I see that passing the `-L` option to the `pwd`
command will return the logical current working directory, while passing
the `-P` option will return the physical current working directory,
which means that all symbolic links will be resolved.  Let's focus on
the options we pass to this command and how the output might differ
depending on what we pass in.

As a side note, it's important to recognize that if no options are
specified, the `-L` option is assumed.

Let's try this out.  I'm going to change into the `/var` directory to
demonstrate:

    cd /var

Next I can run the "print working directory" command:

    pwd

From there I see that the current logical working directory is returned,
which is the `/var` directory:

    /var

Since passing the `-L` option is assumed by default, this command should
return the same value:

    pwd -L

Which it does:

    /var

Let's now pass in the `P` option:

    pwd -P

This time a different result is returned:

    /private/var

It shows that the `/private/var` directory is the physical current working
directory, meaning that the symbolic links have been resolved.

Passing in different options returns different results, which is what one
might expect, but what happens if we pass in both options at the same time?

First, I'll pass in the `L` option, immediately followed by the `P` option:

    pwd -LP

It returns the same directory location as if I had only passed the `P` option:

    /private/var

However, if we swapped the options and called it with the `P` option first:

    pwd -PL

We'd end up with an entirely different result:

    /var

The takeaway here is that it's helpful to understand what options are
available for a command and which ones won't have any effect whatsoever
if they're used in combination with other options.  

As a side note, if you were paying very close attention, you would have
noticed that I combined both options after using a dash as opposed to
separating them out, like so:

    pwd -P -L

Either syntax is correct, but I prefer to combine them since it's a shorthand
way of expressing the same intent:

    pwd -PL

This is true of many UNIX commands, so please use it where possible, and be
sure to consult the `man` pages if you're unsure of the options or
arguments available for a command that you're using.


## Man page sections

Because there are volumes of information related to UNIX commands, system calls
and other miscellaneous information, the `man` pages are divided into the
following sections:

    1      General User Commands
    2      System Calls
    3      Library Routines (\*)
    4      Special Files and Sockets
    5      File formats and Conventions
    6      Games and Fun Stuff
    7      Miscellaneous Documentation
    8      System Administration
    9      Kernel and Programming Style
    n      Tcl/Tk

The large majority of the commands that you'll use as a beginner can be found in
section 1, which is the "General User Commands" section.  Of course, as you
become more proficient with the system, especially if you become a Systems
Administrator, you'll find that section 8 will come in handy as well.

Other important sections, such as System Calls and Library Routines are reserved
for programming-specific information.

The main reasons for wanting to become familiar with the various man page
sections is not only to understand the overall structure and organization, but
also so you can find additional information about the system.

For example, let's revisit the `man` pages for the `ls` command:

    man ls

Near the very bottom of the documentation, you'll notice a heading
called **SEE ALSO**.  From here there are a number of related commands
listed, including one named `symlink`.  If you look closely, it actually
refers to it as:

    symlink(7)

This means that we should review section 7, **Miscellaneous Documentation**,
for further information.  Let's do that:

    man 7 symlink

That's the same syntax as we've used before for looking up man page,
with the exception that this time we're specifying the man page section
before the command.

As you can see, this man page is devoted to describing what a "symlink" is, as
well as some history and common uses for it.

Let's contrast this with the conventional way of looking up documentation for
the `symlink` command, as follows:

    man symlink

This time, the documentation is much different, as it shows
programming-specific details such as the header file where this command
is defined for the C programming language.  There is also
a **DESCRIPTION** of what this library call does, it's possible return
values, as well as common **ERROR CODES** that you might find important
if you were using this function call while programming.

The important part to remember here is that some commands or system
calls can be found in multiple sections of the man page documentation,
and sometimes specifying the section number is crucial to getting the
exact documentation that you need.


## The whatis command

Let's now turn our attention to a couple of utilities that can help you
find more information about the commands you'll be using on your system.

The first utility is the `whatis` command.  Let's take a closer look at it:

    man whatis

As the man page indicates, the `whatis` command "...searches a set of database
files containing short descriptions of system commands for keywords and displays
the result on the standard output".

Let's give that command a try.  This time, I'll do so by looking up the
`ls` command:

    whatis ls

As you can see, it shows a number of results that all have `ls` within their
command name.  I can look at the corresponding descriptions and use the man
pages for those commands shown to see if there is further information that would
be helpful to my particular problem.


## The apropos command

The next utility I'd like to show is the `apropos` command.  Again, let's
reference the man page to find out more details:

    man apropos

As it's man page indicates, the `apropos` command works similar to the
`whatis` command.  However, it is a much more feature-rich command than
the man page indicates, because it also searches through the man page
abstracts, man page titles and will also return results for matching
only a part of a word.

Let's try the `apropos` command using `ls` as an argument:

    apropos ls

As you can see, the results are much more verbose than when we used the
`whatis` command.  Just keep in mind that if you can't find what you
need through the `whatis` command, you can always use the `apropos`
command as an alternative.

So that's it for the UNIX `man` pages.  As you become more experienced
in your UNIX knowledge, you'll find the `man` pages to be one of the
most useful tools on the system.  Before you launch that web browser and
start searching the Internet, please try out the `man` pages as well as
the `whatis` and `apropos` commands to help you find what you need.

## The help command

In the `bash` shell, the `help` utility can be useful for finding help
for some of the shell's "builtin" commands.  By default, typing `help`
without arguments lists those commands:

    iMac:~ screencast$ help
    GNU bash, version 3.2.51(1)-release (x86_64-apple-darwin13)
    These shell commands are defined internally.  Type `help' to see this
    list.
    Type `help name' to find out more about the function `name'.
    Use `info bash' to find out more about the shell in general.
    Use `man -k' or `info' to find out more about commands not in this list.

    A star (*) next to a name means that the command is disabled.

     JOB_SPEC [&]                       (( expression ))
     . filename [arguments]             :
     [ arg... ]                         [[ expression ]]
     alias [-p] [name[=value] ... ]     bg [job_spec ...]
     bind [-lpvsPVS] [-m keymap] [-f fi break [n]
     builtin [shell-builtin [arg ...]]  caller [EXPR]
     case WORD in [PATTERN [| PATTERN]. cd [-L|-P] [dir]
     command [-pVv] command [arg ...]   compgen [-abcdefuv] [-o option
     complete [-abcdefuv] [-pr] [-o continue [n]
     declare [-afFirtx] [-p] [name[=val dirs [-clpv] [+N] [-N]
     disown [-h] [-ar] [jobspec ...]    echo [-neE] [arg ...]
     enable [-pnds] [-a] [-f filename]  eval [arg ...]
     exec [-cl] [-a name] file [redirec exit [n]
     export [-nf] [name[=value] ...] or false
     fc [-e ename] [-nlr] [first] [last fg [job_spec]
     for NAME [in WORDS ... ;] do COMMA for (( exp1; exp2; exp3 )); do COM
     function NAME { COMMANDS ; } or NA getopts optstring name [arg]
     hash [-lr] [-p pathname] [-dt] [na help [-s] [pattern ...]
     history [-c] [-d offset] [n] or hi if COMMANDS; then COMMANDS; [ elif
     jobs [-lnprs] [jobspec ...] or job kill [-s sigspec | -n signum | -si
     let arg [arg ...]                  local name[=value] ...
     logout                             popd [+N | -N] [-n]
     printf [-v var] format [arguments] pushd [dir | +N | -N] [-n]
     pwd [-LP]                          read [-ers] [-u fd] [-t timeout] [
     readonly [-af] [name[=value] ...]  return [n]
     select NAME [in WORDS ... ;] do CO set [--abefhkmnptuvxBCHP] [-o opti
     shift [n]                          shopt [-pqsu] [-o long-option] opt
     source filename [arguments]        suspend [-f]
     test [expr]                        time [-p] PIPELINE
     times                              trap [-lp] [arg signal_spec ...]
     true                               type [-afptP] name [name ...]
     typeset [-afFirtx] [-p] name[=valu ulimit [-SHacdfilmnpqstuvx] [limit
     umask [-p] [-S] [mode]             unalias [-a] name [name ...]
     unset [-f] [-v] [name ...]         until COMMANDS; do COMMANDS; done
     variables - Some variable names an wait [n]
     while COMMANDS; do COMMANDS; done  { COMMANDS ; }

For specific help on any of those commands listed, just type `help`
followed by the shell builtin.  As an example, here I'd like to get help
for the `export` builtin function:

    iMac:~ screencast$ help export
    export: export [-nf] [name[=value] ...] or export -p
        NAMEs are marked for automatic export to the environment of
        subsequently executed commands.  If the -f option is given,
        the NAMEs refer to functions.  If no NAMEs are given, or if `-p'
        is given, a list of all names that are exported in this shell is
        printed.  An argument of `-n' says to remove the export property
        from subsequent NAMEs.  An argument of `--' disables further option
        processing.

And here's another example, this time describing the help for the
`unset` builtin:

    iMac:~ screencast$ help unset
    unset: unset [-f] [-v] [name ...]
        For each NAME, remove the corresponding variable or function.  Given
        the `-v', unset will only act on variables.  Given the `-f' flag,
        unset will only act on functions.  With neither flag, unset first
        tries to unset a variable, and if that fails, then tries to unset a
        function.  Some variables cannot be unset; also see readonly.


## The info command

In recent years, there has been another alternative to the UNIX `man` pages.  This
system is called `info`, and it refers to documentation in `info` or
**Texinfo** format.  It is based upon **GNU emacs**, which is popular text
editor among programmers. 

Let's give it a try:

    info ls

As you can see, it provides output very similar to the `man` pages, but
navigating the system is much different than with the default system
pager, such as `less` or `more`.  If you're an `emacs` user, you'll
probably find the navigation to feel more natural.

At this time, the `info` pages are not as widely adopted or common as
the UNIX `man` pages, but I encourage you to try them out as an
alternative source of system information.  


## Chapter summary

That wraps up this chapter on **Getting Help**.  We've looked at man
page structure and style, as well as several ways to find details about
command usage.  We've also reviewed a few ways to find out about other
commands on your system.  

If you'd like to find out more information on man pages, please lookup
the documentation for the `manpages` command:

    man manpages

