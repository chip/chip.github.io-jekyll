# Redirection and Pipes

One of the most important features of a UNIX operating system is it's
ability to manage streams of data and redirect it to an appropriate
destination.  

Having the ability to take the output of one command and feed it as the
input to another command is one of the hallmarks of UNIX.  It's part of
what makes the operating system so remarkably powerful, and allows
programmers and systems administrators alike to build more complex
applications by chaining together many smaller, more focused utilities.
It's one of the main reasons that they are able to "write programs that
do one thing and do it well", as we discussed in a previous chapter.

<!-- Show slide with that quote -->

In this section we'll uncover the various ways that you can use this
feature of UNIX to capture, redirect and filter data.


## UNIX streams

The way UNIX manages streams of data is by using file descriptors, which
are essentially integers that identify open files.  Every UNIX operating
system has access to at least 3 common file descriptors: standard input,
standard output and standard error.

Here's an overview of those files descriptors and their locations on the
system:

    File descriptor          value  locations
    ---------------------    -----  ---------
    Standard input  (stdin)    0    /dev/stdin,  /dev/fd/0
    Standard output (stdout)   1    /dev/stdout, /dev/fd/1
    Standard error  (stderr)   2    /dev/stderr, /dev/fd/2

Standard input is data that is read into a program.  A common example of
standard input is reading data that a user types from a keyboard.  Of
course, standard input can also be accepted from commands and utilities,
as we'll seen in a moment.

Standard output is data that is written from a program.  For example,
when a command writes data to the screen or terminal, that is considered
to be standard output.  Of course, that data doesn't have to be written
to the screen, as it can also be redirected to other programs.

Standard error is basically the errors that a program outputs, such as
if you pass the wrong arguments to a command, it will issue a warning or
error that is written to standard error, which in many case is the
screen or terminal.

To fully grasp these concepts, let's look at the various ways that we
can use these standard data streams.


## Redirect stdout

### Using cat to input data

First, I'm going to use the `cat` command and redirect its standard
output to a file called `fruits`.

    iMac:~ screencast$ cat > fruits

As you can see, I'm using the greater-than `>` symbol to redirect the
data.  To allow this to sink in a bit, it might help you to think of the
greater-than `>` symbol as an arrow that shows what direction the data
will flow.

    apple
    orange
    banana

    Ctrl-d

I can then type as much in as I want, but I'll need to type `Ctrl-d`
when I'm done to trigger the file to be written.  Now I can display the
contents of the file using `cat` again:

    iMac:~ screencast$ cat fruits 
    apple
    orange
    banana


### Redirect standard output from a command to a file

Let's now send the standard output of one command to a file.  Reviewing
the fruits file, we can see we still have some data to work with:

    iMac:~ screencast$ cat fruits 
    apple
    orange
    banana

I'm going to revisit a command that we've used previously for displaying
some data from this file.  That command is `head`, which normally
displays the first 10 lines of a file, but you can also pass the `n`
option to specify how many lines you'd like to show.  In this case,
I just want the first line of the file to be shown:

    iMac:~ screencast$ head -n1 fruits 
    apple

As you can see, it wrote the line containing apple to the screen.  Now
let's redirect that output to a file using our redirection operator:

    iMac:~ screencast$ head -n1 fruits > fruits-2

Notice how this time nothing was written to the screen?  That's because
the standard output was redirected to the fruits-2 file, instead of the
screen which is its default location.  

Let's review that newly created file:

    iMac:~ screencast$ cat fruits-2
    apple

As you can see, the output from `head` was indeed written there.


### Using cat with multiple file arguments

In this example, I'm going to create a couple of files quickly by
echoing a string and redirecting the standard output to those new files:

    iMac:~ screencast$ echo "strawberry" > fruits-3
    iMac:~ screencast$ echo "raspberry" > fruits-4

So let's review those files:

    iMac:~ screencast$ cat fruits-2
    apple
    iMac:~ screencast$ cat fruits-3
    strawberry
    iMac:~ screencast$ cat fruits-4
    raspberry

Since the `cat` commands' specialty is to concatenate files, it can
accept multiple file arguments.  I'm going to pass those new files to it
and redirect its standard output to the fruits-5 file.

    iMac:~ screencast$ cat fruits-2 fruits-3 fruits-4 > fruits-5

Using `cat` to display its contents, we can see that all of the content
from those 3 files has been written to the new file:

    iMac:~ screencast$ cat fruits-5
    apple
    strawberry
    raspberry


### Redirecting with > can create, but also overwrites

One point that I'd like to stress about this type of redirection is that
it not only can create new files, but it will also completely overwrite
the file if it already exists.

Let's take a look at an example.  First I'm going to copy an existing
file to another file called fruits-6 and take a look at its contents:

    iMac:~ screencast$ cp fruits-5 fruits-6
    iMac:~ screencast$ cat fruits-6
    apple
    strawberry
    raspberry

Then I'm going to using the greater-than `>` symbol to redirect the
standard output like we've seen before and enter some data:

    iMac:~ screencast$ cat > fruits-6
    pineapple

I'll type `Ctrl-d` to finish entering the data, so that it will write to
fruits-6.  However, since the file already existed, it should simply
overwrite it with the new contents.  Let's take a look to be sure:

    iMac:~ screencast$ cat fruits-6
    pineapple

When using this type of redirection, use care and be sure that
you're ok with overwriting the file if it does already exist.


## Redirect stdout with append operator: >> 

Let's say that you don't want to overwrite a file, but instead simply
want to append the standard output to the end of a file.  In that case,
you can just use the append redirection operator, which is represented
using 2 greater than symbols `>>`.  Let's look at an example to see this
in practice.  First, we'll display the contents of the fruits file:

    iMac:~ screencast$ cat fruits
    apple
    orange
    banana

Then we'll follow an example similar to what we tried before with the
`cat` command, but this time we'll use the append redirection operator.
I'll type in some data, then just like before I'll type Ctrl-d when I'm
done:

    iMac:~ screencast$ cat >> fruits
    peach
    grape
    tangerine
    cherry

Let's now take a look at that file:

    iMac:~ screencast$ cat fruits
    apple
    orange
    banana
    peach
    grape
    tangerine
    cherry

This time, instead of overwriting the file, the data was simply appended
to the fruits file.


## Redirect standard output of multiple files with append operator: >>

As another example, we can redirect standard output using the append
operator, but this time do so using multiple files.  Let's revisit the
contents of a few of the fruits files:

    iMac:~ screencast$ cat fruits-2
    apple
    iMac:~ screencast$ cat fruits-3
    strawberry
    iMac:~ screencast$ cat fruits-4
    raspberry

And then we can take the concatenated contents of these files, in the
order that they're listed and redirect it to the fruits-4 file, this
time appending the data to what is already there:

    iMac:~ screencast$ cat fruits-2 fruits-3 >> fruits-4

Then we can review the contents of the destination file:

    iMac:~ screencast$ cat fruits-4
    raspberry
    apple
    strawberry

We can see that data from the other files was indeed appended to
fruits-4.

As a side note, the append operator also works on files that don't
exist.  Again we can concatenate a couple of files to a file called
somefile, and it will be created with the contents of those files passed
as arguments to the `cat` command:

    iMac:~ screencast$ cat fruits-2 fruits-3 >> somefile
    iMac:~ screencast$ cat somefile 
    apple
    strawberry


## Redirect stdin using the < operator

Here we'd like to shift our attention to redirecting standard input.
This can be done with the operator that looks like a less-than symbol,
`<`.

### mail

In this example, I'm going to send an email to myself, but have the
contents of the fruits file sent to the command by redirecting standard
input:

    mail chip@chipcastle.com < fruits

This is easier than copying and pasting the contents of the file into
the body of the email message, so it's a bit of a time-saver.

Additionally, you can take the contents of a file and use it as standard
input for other commands, like `cat`:

    iMac:~ screencast$ cat < fruits
    apple
    orange
    banana
    peach
    grape
    tangerine
    cherry

This isn't too terribly useful since `cat` already accepts file
arguments, like this:

    iMac:~ screencast$ cat fruits
    apple
    orange
    banana
    peach
    grape
    tangerine
    cherry

However, for any command that doesn't accept file arguments, such as the
`mail` command shown previously, this is a useful trick to be aware of.



## Redirect stdin, then stdout

Sometimes it's useful to redirect standard input and output on the same
command.  Let's revisit the contents of our fruits file:

    iMac:~ screencast$ cat fruits
    apple
    orange
    banana
    peach
    grape
    tangerine
    cherry

Let's say I want to sort this file, but I also want to save the contents
to a separate file altogether.  This can easily be done using both
redirection operators:

    iMac:~ screencast$ sort < fruits > fruits.sorted

What this does is first send the contents of the fruits file to the
`sort` command, which is it's standard input.  Then, it sorts the file
accordingly and sends it standard output to the fruits.sorted file. 

Take note that the output was not displayed to the terminal.  Instead,
the results can be found inside the new file.  Now we can view the
results using `cat`:

    iMac:~ screencast$ cat fruits.sorted 
    apple
    banana
    cherry
    grape
    orange
    peach
    tangerine

As you can see, the results were sorted alphabetically, just as we
specified on the command line.


## Redirect stdout and stderr

As we've discussed earlier, standard output and standard error by
default are separate locations.  Let's use an example to help clarify
this concept.  I'm going to get a file listing of 2 files, except that
one of them doesn't exist:

    iMac:~ screencast$ ls -l fruits vegetables
    ls: vegetables: No such file or directory
    -rw-r--r--  1 screencast  staff  49 Jul 29 16:12 fruits

Here we see an error for the non-existent vegetables file, as well as
file details for existing the fruits file.  This seems normal, but
there's something going on behind the scenes that might not be
immediately apparent.

Watch as I try the same example, and then redirect the standard output
of this command to a file called output.txt:

    iMac:~ screencast$ ls -l fruits vegetables > output.txt
    ls: vegetables: No such file or directory

This time, we only see the error.  This is standard error and is to be
expected, because we did not redirect it.  However, we did redirect
standard output, so we should see the results from the `ls` command
within that file:

    iMac:~ screencast$ cat output.txt 
    -rw-r--r--  1 screencast  staff  49 Jul 29 16:12 fruits

Let's say that we'd like standard output and standard error to be sent
to the same file.  Here is how it's done:

    iMac:~ screencast$ ls -l fruits vegetables &> output.txt

Taking a look at the output.txt file again, we see that indeed the
standard error and standard output were redirected there.

    iMac:~ screencast$ cat output.txt 
    ls: vegetables: No such file or directory
    -rw-r--r--  1 screencast  staff  49 Jul 29 16:12 fruits

Using the `&>` shortcut is just one of the ways to accomplish this type
of redirection.

  Side note: Be sure to type the ampersand, immediately followed by the
             greater than symbol, with no whitespace in between
    

There's another way to follow this technique, so let's first clean up
our output file:

    iMac:~ screencast$ rm output.txt 

Now we can issue the alternative format, although it is a bit more
verbose:

    iMac:~ screencast$ ls -l fruits vegetables > output.txt 2>&1

So what does this do?  It first redirects standard output to output.txt.
Notice the special notation that's being used here:

    2>&1

This is the same as saying this:

    x>&y

Which means, "redirect file descriptor x to file descriptor y".  Keep in
mind that x is standard output (or 1) unless specified.  Since it is
specified as 2, we know that it is redirecting standard error to
standard output.

Based on the `ls` command we tried, we know that standard output was
already being redirected to the file output.txt, so this is where
standard error should be sent as well.

And taking a look at the file again, we see that this accomplished the
same result as before:

    iMac:~ screencast$ cat output.txt 
    ls: vegetables: No such file or directory
    -rw-r--r--  1 screencast  staff  49 Jul 29 16:12 fruits

Again, please bear in mind the proper syntax does not allow for spaces:

    2>&1

Additionally, notice that the following example means something
completely different:

    2>1

This says to take standard error and redirect it to a file named 1, not
file descriptor 1, which is the standard output.


## Redirecting stdout and stderr to separate locations

Sometimes it would be nice to redirect both standard output and standard
error, but we'd prefer to have the results sent to separate locations.
First I'll clean up our old output files:

    iMac:~ screencast$ rm errors.txt output.txt 

Then we can try an example:

    iMac:~ screencast$ ls -l fruits vegetables > output.txt 2> errors.txt

The first part of the `ls` command says to send the standard output to
the output.txt file.  The next part is using the special syntax `2>`,
which says to redirect standard error to errors.txt.

Now let's take a look at the output file:

    iMac:~ screencast$ cat output.txt 
    -rw-r--r--  1 screencast  staff  49 Jul 29 16:12 fruits

We can see that it only shows the results from `ls`.

And now let's look at the errors.txt file:

    iMac:~ screencast$ cat errors.txt 
    ls: vegetables: No such file or directory

Here we see that it only shows the error, which is exactly what we
intended.


## Redirect stdout to nowhere

Let's say I have a program that generates an error and I'm not
interested in seeing it.  I can send that error to a place commonly
referred to as a bitbucket, which is `/dev/null`.

If my program were to do something like check for a process id, I could
do something like this:

    iMac:~ screencast$ cat /var/run/syslog.pid 
    19

Since that file exists, it displays the contents to my terminal.

However, let's try the same example with a file that does not exist:

    iMac:~ screencast$ cat /var/run/does-not-exist.pid 
    cat: /var/run/does-not-exist.pid: No such file or directory

This shows an error, of course, but if I don't want to see that output
I'll need to redirect standard error somewhere.  

Let's try this:
    iMac:~ screencast$ cat /var/run/does-not-exist.pid 2> /dev/null
    iMac:~ screencast$ 

This says to display the contents of the file to the terminal, but if it
runs into any errors, to just send them to the null device.  Basically
it just discards the errors.


### Example of a "here" document 

Let's say you want to test out a command using some sample text, but
want to do so without opening a text editor.  Basically what you need is
a temporary document.  In UNIX-speak this is more commonly referred to
as a "here" document.

Let's create one with the `cat` command to demonstrate the idea.

    iMac:~ screencast$ cat <<EOF

After typing `cat`, I'll use 2 less-than symbols `<<` immediately
followed by a string of uppercase characters.  In this example, I used
`EOF`, but I could've used any string of uppercase characters as
a terminator for the input stream.  This tells `cat` to allow me to type
in data until it finds that terminator at the beginning of a line.  I'll
then type in some text followed by that `EOF` terminator:

    > This is a "here" document.
    > It preserves line breaks,
    >   tabs  and   spaces.
    > It also does environment variable expansion, such as:
    > My default shell is $SHELL
    > And also command substitution, such as my account name is `whoami`.
    > Just type the delimiter after << at the beginning of a new line to end
    > this.
    > EOF
    This is a "here" document.
    It preserves line breaks,
      tabs  and   spaces.
    It also does environment variable expansion, such as:
    My default shell is /bin/bash
    And also command substitution, such as my account name is screencast.
    Just type the delimiter after << at the beginning of a new line to end
    this.

As you can see, it maintains the formatting, expanded the `$SHELL`
environment variable, and ran the `whoami` command.


## Pipes

In this section we'll cover how to use UNIX pipes, which in my opinion
is one of the most important and powerful facets of the operating
system.

The concept of pipes is quite simple:

    "The output of one command is sent as the input to another command"

That use of pipes is represented by the vertical bar character `|`.

Let's revisit our "here" document example from the previous section.

I'll use the `cat` command to create the document, but at the end of the
line I'll use a pipe symbol to indicate that the output of this command
should be fed as input to the `sort` command:

    iMac:~ screencast$ cat <<MUSIC | sort
    > drum and base
    > trance
    > dub step
    > house
    > MUSIC
    drum and base
    dub step
    house
    trance

As you can see, the contents of the "here" document were sorted.

Let's take a look at another simple example.  I'll issue an `ls` command
to see the contents of the current directory.

    iMac:~ screencast$ ls -l
    total 80
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwsrwsrwt+  4 screencast  staff   136 Jul 28 13:20 Public
    -rw-r--r--   1 screencast  staff    42 Jul 31 17:08 errors.txt
    -rw-r--r--   1 screencast  staff    49 Jul 29 16:12 fruits
    -rw-r--r--   1 screencast  staff     6 Jul 29 14:24 fruits-2
    -rw-r--r--   1 screencast  staff    11 Jul 29 14:52 fruits-3
    -rw-r--r--   1 screencast  staff    27 Jul 29 16:39 fruits-4
    -rw-r--r--   1 screencast  staff    27 Jul 29 14:56 fruits-5
    -rw-r--r--   1 screencast  staff    10 Jul 29 16:16 fruits-6
    -rw-r--r--   1 screencast  staff    49 Jul 31 16:51 fruits.sorted
    -rw-r--r--   1 screencast  staff  1044 Jul 31 17:23 ls.out
    -rw-r--r--   1 screencast  staff    17 Jul 29 16:40 somefile

That's way more output than I'd really care to see.  What if I wanted to
pare down this output and only show the files that contained the string
"fruit".  I could do that simply by using a pipe.  Here I'm going to
pass the output of the `ls` command to a separate command where I `grep`
for the string containing "fruit":

    iMac:~ screencast$ ls -l | grep fruit
    -rw-r--r--   1 screencast  staff    49 Jul 29 16:12 fruits
    -rw-r--r--   1 screencast  staff     6 Jul 29 14:24 fruits-2
    -rw-r--r--   1 screencast  staff    11 Jul 29 14:52 fruits-3
    -rw-r--r--   1 screencast  staff    27 Jul 29 16:39 fruits-4
    -rw-r--r--   1 screencast  staff    27 Jul 29 14:56 fruits-5
    -rw-r--r--   1 screencast  staff    10 Jul 29 16:16 fruits-6
    -rw-r--r--   1 screencast  staff    49 Jul 31 16:51 fruits.sorted

The result of that command only showed lines matching the pattern we
specified.

Take a moment and think about this idea.  It is extremely powerful, as
it allows you to chain together multiple smaller commands and utilities,
with each command being used to filter the previous command's output.


### Example from "Top 10 UNIX commands"

In this example, I'll show a more practical use of pipes that has been
very useful in my own workflow.  I need to credit Ben Orenstein `@r00k`
for the original idea.  Since I first heard of it, I've posted this tip
on my free UNIX newsletter at http://unix.chipcastle.com and received
a really positive response.  Based on that feedback, I've come up with
a variation of this tip, which I'll show here.

So what I'd like to do is find out the **Top 10 UNIX commands** that
I currently use.  Having this information is valuable because I can then
create short aliases for these commands, thus I can speed up my workflow
because I'll type much fewer keystrokes.  It might not sound like a big
improvement at first, but if you type certain commands dozens or
hundreds of times a day, the time savings can really add up over the
long term, not to mention the reduced wear and tear on your fingers.

<!--
  The output will be truncated for most of the commands that follow in
  the accompanying documentation.  Please see the screencast for full
  output if you're interested.
-->

To start off, we'll issue the `history` command to see everything that
we've typed into the shell:

    iMac:~ screencast$ history
    744  ls -l hello
    745  chmod +s hello
    746  ls -l hello
    747  ./hello
    748  man chmod
    749  ls -l
    750  chmod a= hello
    751  ls -l hello
    752  chmod +s hello
    753  ls -l hello

We really aren't interested in the number associated to the left of each
command's output, so I'm going to pipe the results of `history` to the
`cut` command, which according to the UNIX man pages is used to "cut out
selected portions of each line of a file":

    iMac:~ screencast$ history | cut -c8-
    chmod +s hello
    ls -l hello
    ./hello
    man chmod
    ls -l
    chmod a= hello
    ls -l hello
    chmod +s hello
    ls -l hello
    chmod a+x hello

Here we instruct the `cut` command to remove only the first 8 characters
of the output using the `-c` flag.  Now we just see the command itself
returned in the results, so we're making progress.

Now all I'm interested in is the command itself, not any options, flags
or arguments, so I'll need to use `cut` again to filter the data
a little more:

    iMac:~ screencast$ history | cut -c8- | cut -d ' ' -f1
    chmod
    chmod
    ls
    chmod
    chmod
    chmod
    ls
    chmod
    ls
    ./hello

Here we see the `-d` flag specifies that I should use a single space as
the field delimiter so that it knows where to divide the data.  

The `-f` flag instructs `cut` to return only the first field of the
 data which is split on a space.

Next, we simply sort the output alphabetically:

    iMac:~ screencast$ history | cut -c8- | cut -d ' ' -f1 | sort
    ./hello
    cal
    cal
    cat
    cat
    cat
    cat
    cat
    cat
    cat

Since there are a lot of commands that we use over and over, our output
has a lot of duplicates.  We can filter those sorted results using
`uniq`, but we must include the `-c` option to precede each output line
with the count of the number of times the line occurred in the output.

    iMac:~ screencast$ history | cut -c8- | cut -d ' ' -f1 | sort | uniq -c
       1 
       1 ./hello
       2 cal
      68 cat
       2 cd
      80 chmod
       1 cp
       1 crontab
       5 echo
       1 env

To round out the most frequently used commands, we need to `sort` the
output numerically due to the command counts provided by `uniq`, and to
be sure to do so in reverse order since we’re interested in the most
frequently occurring commands first:

    iMac:~ screencast$ history | cut -c8- | cut -d ' ' -f1 | sort | uniq -c | sort -nr
     176 ls
      79 chmod
      68 cat
      34 rm
      33 man
      20 touch
      19 sudo
      18 history
       9 for
       5 rmdir

Lastly, we use `head` to show the top 10 shell commands. 

    iMac:~ screencast$ history | cut -c8- | cut -d ' ' -f1 | sort | uniq -c | sort -nr | head
     176 ls
      79 chmod
      68 cat
      34 rm
      33 man
      20 touch
      19 sudo
      18 history
       9 for
       5 rmdir

Of course, you could always vary this a bit if you’re interested in more
or less than 10 by simply specifying `-n` option for `head`.

This example is fairly straightforward and only touches the tip of the
iceberg when it comes to the true power of using pipes.  One of the
central ideas of UNIX is to create small programs that, "do one thing
and do it well".  By using pipes, it allows us to do just that and chain
together a wide array of commands to build more complex, sophisticated
applications while doing so in a flexible manner.

This concept also encourages old and new UNIX users alike to always
learn new commands.  The more commands you have in your tool belt, the
more options you have at your disposal when building software or
maintaining a system. As a software developer, I've always felt that one
of the most important things I could do is to reuse software that has
already been written.  By doing so, I save myself time designing,
writing, testing and debugging.  It also allows me to focus on what
I really want to accomplish.


### tee

To wrap up this chapter, I wanted to show you a technique that combines
the idea of output redirection with the functionality of UNIX pipes.

Let's say you're interested in how much free disk space you have on your
system.  

That easy enough `df` command, which is used to "display free disk
space".  Passing the `-h` option returns human-readable output of the
disk usage stats:

    iMac:~ screencast$ df -h
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  152Gi   81Gi    66% 39786839  21282601   65%  /
    devfs          201Ki  201Ki    0Bi   100%      696         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

But what if we also want to save those results to a file?

That would be easy to do as well, just by redirecting standard output
(i.e., stdout) to a file:

    iMac:~ screencast$ df -h > df.out

**That works fine, except that I don't want to type 2 separate
commands.**

In fact, for some examples, it might not be feasible to run the commands
separately because the output might differ.  What you need instead is
the ability to capture the output, send it to standard output (e.g., as
in the terminal), and also save it to a separate file.  

Well, there's a command that will help you do just that - the `tee`
command.  

**According to the UNIX man pages on OS X:**
*The tee utility copies from standard input to standard output, making
a copy in zero or more files.*

Let's try our example again, but this time using tee:

    iMac:~ screencast$ df -h | tee df.out
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  151Gi   82Gi    65% 39657219  21412221   65%  /
    devfs          204Ki  204Ki    0Bi   100%      704         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

Now let's take a look at the contents of that file, just to be sure that
the disk information was written there in addition to what we saw on
standard output:

    iMac:~ screencast$ cat df.out 
    Filesystem      Size   Used  Avail Capacity  iused     ifree %iused Mounted on
    /dev/disk0s2   233Gi  151Gi   82Gi    65% 39657219  21412221   65%  /
    devfs          204Ki  204Ki    0Bi   100%      704         0  100%  /dev
    /dev/disk1s2   931Gi   91Gi  840Gi    10% 23782594 220324072   10%  /Volumes/Macintosh HD 2
    map -hosts       0Bi    0Bi    0Bi   100%        0         0  100%  /net
    map auto_home    0Bi    0Bi    0Bi   100%        0         0  100%  /home

The output is identical to what we saw when we ran the original command,
which is exactly the result we want.

So, next time you need to see standard output and also copy it to
a file, remember that the `tee` command is a great option.


## Chapter summary

So that wraps up this chapter.  Here we've covered various concepts,
including:

<!--

  Use presentation slides here

-->

    File descriptors:
    stdin
    stdout
    stderr

    Redirection techniques:
    >
    >>
    <
    &>
    2>
    2>&1
    "here" documents

    Usage of Pipes:
    |
    tee
    
I challenge you to go try all of these techniques in your everyday
workflow, as it will allow you to become more proficient in your skills 
and have better knowledge of how to use UNIX in more effective ways.
