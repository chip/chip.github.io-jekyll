# Managing files and directories

Being able to work with files and directories is essential to using the
UNIX operating system.  We'll start off with a basic overview before
diving into specific commands for manipulating the file system.


## Directory hierarchy

In UNIX, as in many other operating systems, a file system is a collection of
directories and files that are organized in a tree-like structure.

The following diagram is a sample directory structure on my local system:

<!-- Slide needed -->
                                                    /
        ______________________________________________________________________________________________
        /            /            /             /        /       /     /     /     /     /     /     /
    Applications  Library   Network System    Users   Volumes   bin   dev   etc   sbin  tmp   usr   var
                                                ^ 
                              _________________________________
                              /           /         /         /
                            Shared      admin     chip    screencast
                                                              ^
                                 ____________________________________________________________
                                 /              /             /              /              /
                              Desktop        Downloads      Library        Music          Public


Each directory is a folder-like structure that can contain files or
other directories files beneath it.  At the top we have the "root
directory", followed by all of the directories located immediately under
it.  Drilling down further, this time only showing the contents of the
`/Users` directory, we see 4 directories: `Shared`, `admin`, `chip` and
`screencast`.  The last directory is for the **screencast** user,
showing that it contains a directory for `Desktop`, `Downloads`,
`Library`, `Music` and `Public`.


## OS X directory layout

Here's the basic organization of the directory hierarchy for OS X:

    /             - Known as the parent directory of all directories.  AKA the "root directory".
    /Applications - Where your applications are kept
    /Library      - System-wide shared libraries, settings & preferences
    /Network      - Contains network-related devices, libraries and so on
    /System       - Related system files and libraries
    /Users        - Where all user account files are located
    /Volumes      - Any attached or "mounted" devices (e.g., external drives, Disk images, other system drives)
    /bin          - Executable files essential for booting the system and running properly
    /dev          - The location of all devices such as trackpads, mice and keyboards
    /etc          - Contains system configuration files and settings for various servers or daemons
    /sbin         - Contains systems administration specific binaries
    /tmp          - Temporary files
    /usr          - Other essential operating system files and configuration
    /var          - Contains data that changes often such as log files

Take a moment to familiarize yourself with these directories, as each
one has a specific name and purpose that helps keep related files
organized on your system.


## Pathnames and ls

To fully understand this hierarchy, you'll have to grasp what
**pathnames** are and how they're used.  To do this, let's get a listing
of contents of the root directory using the `ls` command:

    ls /

As you can see, a number of directories that we previously described are listed here.

Using the `-1` option, we are able to see the data in an alternative format that
displays on a single column for readability:

    ls -1 /

But let's dig a little further.  Let's say I wanted to view the contents of
the Users directory.  I know it is available immediately off the root directory,
so I would do this:

    ls /Users

Notice how I've used a forward-slash **/** character, followed by the name of
the directory.

Looking at those results, I see that the **screencast** account is
located beneath it.  If I wanted to see the contents of that directory,
I would need to use this command:

    ls /Users/screencast

Do you see a pattern here?  Basically, directories are separated by a forward
slash **/** character.

By default, the output from the `ls` command is only 1-level deep, but I can
view more files and directories within the tree structure by using the `-R`
option, which means to recursively list everything under the directory
I specify, like so:

    ls -R /Users/screencast

That command resulted in an enormous amount of information displayed to
the screen, so obviously that directory contains a complex directory
hierarchy.

Let's look at another option for the `ls` command, which is the **long format**:

    ls -l /Users/screencast/Documents

This is one of my personal favorites because it packs a lot of
information into a single option.  Taking a look at the output, we can
see that we have permissions listed in the first column, followed by the
number of links, which I'll explain a bit later, then the owner of the
file or directory, the group it belongs to, the date and time of when it
was modified last, and finally the name of the entry.


## File types

Notice the first character on each line, which designates what type of
file it is.  To understand the different UNIX file types, let's take
a look at the following table:

           b     Block special file.
           c     Character special file.
           d     Directory.
           l     Symbolic link.
           s     Socket link.
           p     FIFO.
           -     Regular file.

Underneath the `/Users/screencast/Documents` directory we have
a number of **Directories**, specified with the **d** character, as well as a number
of **Regular files**, as indicated by the dash character **-**.

For other examples, we can look at the `dev` directory for various devices:

    ls -l /dev

Within that directory, there are a few **Block special files**, in addition to numerous
**Character special files**. 

In the following directory, we can see examples of the **Socket link** files:

    ls -l /private/var/run

And here we can see an example of a **Symbolic link** file:

    ls -l /private/etc

What these files do isn't important at this stage, but rather that each file in
UNIX has a special category or designation that it belongs to, and by using the
`ls` command, we can determine what those values are for a given file or
directory.


## The touch command

Before we go any further into manipulating files in this chapter, I want to
cover a quick and easy way to create files.  Now, there are a number of ways to
do this from the command line, but using the **touch** command is one of the
easiest.

    touch drink

Now let's take a look at the directory contents again:

    ls -al

Using the **long** format and showing **all** files with the `a` option,
we see the dot directories and also the **drink** file we just created.
It's an empty file, so there are no contents to show, but it will at
least allow us to demonstrate some of the upcoming commands for
manipulating files.

Let's create a few more files using the **touch** command so that we can see how
using "Wildcards" can be useful:

    touch drinking drank drunk dunk


## Wildcard basics


### The Star

The first wildcard that we'll cover is the Star, which is represented by an
asterisk character: \*.  It can be used to match any number of characters, so if
we were to specify it like this:

    ls drink*

The results would be any file that starts with the pattern "drink", which as follows:

    drink    drinking 

Alternatively, we could use it to match files that match any ending
pattern, such as a file suffix.  Here I'll create a few more files to
demonstrate:

    touch 1.html 2.html 3.htm

Now, if I used the following pattern, it should show me any files that end with
a period, followed by the letters "html":

    ls  *.html

And here's the result:

    1.html   2.html

I can also combine these wildcards, as in this example:

    ls *.htm*

Which means to list any files that end with a period, followed by the
letters "htm" and then **zero or more** of any other characters.

    1.html 2.html 3.htm


### The Question mark

The next Wildcard I'll cover is the Question mark **?**.  This character
will match a single character, which can be anything.

So using the following example:

    ls *.htm?

It says to give me a list of all files ending with a period, followed by
the letters "htm", and then any single character, as shown here:

    1.html   2.html

Notice how it didn't match 3.htm, because that file name doesn't contain
a single character after the "htm" file suffix.

To see another example, let's create 2 files:

    touch fit fat

If I were to use the following pattern:

    ls f?t

It would list both files, because it's looking for any files that start with
"f", followed by any single character, and then end with a letter "t".


### The Square Brackets

The last wildcard that I'll cover is are the Square Brackets **[]**.  This will
match any of the characters that are enclosed within them.

As an example, let's list any files that end in either the letter "l" or the letter "g":

    ls *[lg]

And here are the results:

    1.html   2.html   drinking

And to expand upon that, let's list any files that start with "fo", and end in
either an "a", "b", or "c" character:

    ls fo[abc]

And let's see what matches that pattern:

    foa fob foc

So that wraps up the basics of using Wildcards.  I encourage you to create
a number of files yourself and then experiment with these variations to solidify
your understanding of these rules.


## Making directories

We've already seen that we have directories already on our system, but what if
we want to create a directory on our own?  Well, that can be done using the
**mkdir** command, and then specifying the name of the directory.

So let's say I want to create a "projects" directory.  I can do that like so:

    mkdir projects

But what happens if I try that again?

    mkdir projects

As you can see, the command tells you that the directory already exists.


## Navigating directories

So, now that we have directories, how do we navigate between them?
Well, you can use the **cd** command, which stands for "Change Working
Directory", and is one of the most common commands you'll find on UNIX.
Let's change into the "projects" directory that we just created: 

    cd projects

And then let's look at the contents of this directory:

    ls

That doesn't show much, which I guess is to be expected since all we did was
create an empty directory.

Although they aren't immediately visible, there are 2 special
directories that exist here.  Let's dig a little bit deeper using "ls"
to get a better understanding.


## Special directories: . and ..

This time, let's pass it the **a** option so we can see all of the files, and
let's do that using the **long format** that we've used before so we can see
more details:

    ls -al

As you can see, there are 2 entries.  The first, which is displayed as a single
dot or period is a representation for the current directory, which is the
**projects** directory.  The second entry, which is displayed as two dots or
periods, is a representation for the directory immediately above our current
directory.

By using the **pwd** command, which prints the current working
directory, we can see that we're currently inside the
"/Users/screencasts/projects" directory.  So the single dot represents
that directory, while the double dots represent the "/Users/screencasts"
directory, which is directly above it in the filesystem hierarchy.

Now let's try out these commands:

    cd .

This basically says, "change into the current directory", which basically does
nothing.

However, if we try the double dot version:

    cd ..

It changes us into the directory above, "/Users/screencasts", which is
coincidentally our "home" directory for the username "screencasts".

Now let's go back to the projects directory for a moment:

    cd projects


## Your $HOME directory

If I wanted to go to my home directory, I could always type it in, like so:

    cd /Users/screencasts

However, there's a much more common way to do this, and it requires typing
only a single character, which is the tilde **~** character:

    cd ~

So, if you wanted to go to another accounts directory on the system, you could
prepend the tilde character in front of their account name, as in the case here
where I'm going to the home directory for the "chip" account:

    cd ~chip

So, what happens if we try to change into a directory that doesn't exist?

    cd foo

The response provides an error message, which is helpful.

Another shortcut that I find helpful is using the dash **-** to help me return
to the previous directory.

So let's say I change into my "projects" directory again, which is located under
my home directory:

    cd ~/projects

And later I decide to change into the `tmp` directory, which is where
temporary files are stored:

    cd /tmp

I can always return to the previous directory by just using a dash:

    cd -

And then let's check to see where we're located using the "pwd" command:

    pwd

As you can see, I'm back in my "projects" directory.


## Viewing files

Let's now see how we can view files.  By doing this, we'll be able to
inspect the contents of the files and have a better grasp of what
happens when we move files, copy them, or manipulate them in other ways.

I'm going to create a file called TODO with a few items in a list so
that we can demonstrate these commands.  And for this tutorial, I'm
going to use the `nano` text editor, which is very easy to use and
available on most UNIX systems.  From within nano, the help menu can be
found at the bottom of the screen, which shows the basic commands, for
operations such as opening and saving files, as well as exiting the
application.  The keyboard shortcuts start with a `Control` character,
indicated by a caret symbol `^`, followed by another character.  As an
example, exiting nano requires the `Control-X` keystroke combination.

So now that I've created the file, how can I quickly view it's contents without
opening a text editor?  Let's take a look at a few commands that can
help you do this.


### The cat command

One command that comes to mind is "cat", which is used to view the
contents of a file.   It can also be used to concatenate multiple files
together, which is how it got its name.  For now let's just focus on
using cat to display the contents of a file:

    cat TODO

As you can see, it displays what I had previously typed into that file.

Let's say I have a really large file, and I don't want to see the entire
contents of that file fill up my screen?  In that case, the `cat`
command might be overkill.  What you really need is a way to display the
contents of the file one page at a time.


### The more and less commands

Years ago, I would most likely use the "more" command:

    more TODO

Originally, this command only allowed for forward movement.  Later, a similar
command called "less" also provided the ability to page through the contents of
a file, but also allowed for backward as well as forward movement.  It follows
a similar syntax to "more" - just provide a filename after the command:

    less TODO

Today, you'll find UNIX users more commonly refer to the "less" command.
In fact, on some systems the "more" command is simply linked to the
"less" command.  On my system, I don't find any evidence of that
linkage, but it appears that the commands are exact copies of each
other.  In any event, on most modern systems you might be able to use
the commands interchangeably, but just bear in mind that using "less" is
what you'll commonly see used.


### The head command

Let's now move on to other ways of viewing file contents.  For example,
maybe I just need to see the first few lines of that file.

The easiest way to do that is to use the `head` command:

    head TODO

Notice that by default it only shows the **first 10 lines** of the file, not the
entire file.  Also, I can easily change this default by passing the "n" option
and then a number:

    head -n5 TODO

Here I'm telling the `head` command to display just the first 5 lines of
the TODO file.  Just keep in mind that I can specify any number after
the "n" option.


### The tail command

Alternatively, I can display the **last 10 lines** using another command, called
`tail`:

    tail TODO

The `tail` command also has an "n" option.

    tail -n12

Here I'm specifying the last 12 lines of my TODO file.  However, I could've
specified any number after the "n" option, just like I did with the head
command.


## File details

Now that we've covered some basics about files and directories, I'd like to
cover a couple of utilities that I've found helpful in pinpointing exactly where
a command is located on the system.


### The which command

The first command will locate a program file in the user's path.  That
command is called `which`.  If I were interested in finding where the
`tail` command is located, I could use the `which` command to do that:

    which tail

Which returns `/usr/bin/tail`.

I could also inquire about the `which` command itself, like so:

    which which

This tells me that it's a built-in command, but doesn't tell me the path:

    which: shell built-in command


### The whereis command

However, I can still find the path using another command that will help me
locate programs.  That command is called `whereis`:

    whereis which

This finally gives me the full path to the `which` command:

    /usr/bin/which


### The file command

If I wanted to know a little bit more about this command, I could use
another utility called `file`, specifying the full path to the command.
Using the path name that was returned from the previous `whereis`
command:

    file /usr/bin/which

It tells me the command is an executable file.

    /usr/bin/which: Mach-O 64-bit executable x86_64

This `file` command can be used on directories as well.  Specifying my home
directory using the tilde **~** character:

    file ~

I can see that the path /Users/chip is indeed a directory:

    /Users/chip: directory

Having this information at your disposal will definitely help you have
a better overall picture of the various commands on your system,
including what types of files they are and where they're located.


## Copying files

Now that we know how to navigate around various directories, as well as list
their contents, we might need to copy files around.  So let's look at the basic
syntax:

    cp SOURCE_FILE DESTINATION_FILE_OR_DIRECTORY

We first type in the `cp` command, followed by a source file and then a destination file:

    cp testfile testfile2

After doing that, let's take a look at our directory listing:

    ls -l

We can see as a result of running the `cp` command, that `testfile2` has exactly
the same permissions, ownership and size as the source file called `testfile`.

What if I try copying the file to the same file name?

    cp testfile testfile

You'll get an error because testfile already exists.

Alternatively, we can copy files to directories.  So let's try this out:

    cp testfile /tmp

We just copyied testfile to the `tmp` directory, so let's take a look at
the contents of that directory:

    ls -l /tmp

And there we see that the file was indeed copied there.

But what happens if I were to copy a file to a non-existent directory?
Let's try, but before doing so, let's get a listing of our top-level
directory to see what does actually exist:

    ls -l /

I can see there isn't a directory named "foo", so let's try copying
there just to see what happens:

    cp testfile /foo

As expected, we get an error, because the "foo" directory doesn't exist
under our top-level directory.

<!-- OFF CAMERA -->
  mkdir -p docs/work
  touch docs/1.txt docs/work/{2,3}.txt
<!-- END -->

An additional feature of the copy command includes the ability to copy
an entire directory tree.  Off camera I've created a "docs" directory,
so let's take a look at the contents, adding in the "Recursive" option
so we can see the entire tree of directories and files:

    ls -lR docs 

Looking at the results, we can see that the "docs" directory contains one
file called 1.txt and one directory named "work", and that the
"docs/work" directory only contains two files: 2.txt and 3.txt:

    total 0
    -rw-r--r--  1 chip  staff    0 Jul  9 14:51 1.txt
    drwxr-xr-x  4 chip  staff  136 Jul  9 14:51 work

    docs/work:
    total 0
    -rw-r--r--  1 chip  staff  0 Jul  9 14:51 2.txt
    -rw-r--r--  1 chip  staff  0 Jul  9 14:51 3.txt

So how do I copy the contents of this directory tree?  Just pass the
"Recursive" option to the `cp` command, like so:

    cp -R docs moredocs

Here we're recursively copying the "docs" directory over to the
"moredocs" directory.  Let's take a look:

    ls -lR moredocs

As you can see, it contains an exact copy of all of the directories and
files, preserving their permissions and ownership.


## Moving files

Sometimes instead of copying files around, you simply want to move them.
In UNIX, the `mv` command is similar to `cp` in that you type the
command, followed by a source file, and then finally a destination file
or directory:

    mv SOURCE_FILE DESTINATION_FILE_OR_DIRECTORY

So let's create a few files to see various ways to move them around:

    touch apple banana grape

I have three files now, named `apple`, `banana` and `grape`, respectively.

First, I'm going to move the file named `apple` to the `/private/tmp` directory:

    mv apple /private/tmp

Taking a look at that directory, we can see that it indeed was moved there:

    ls -l /private/tmp

Inspecting our current directory also verifies that the file wasn't simply
copied, as it no longer exists there:

    ls -l

Next, I'm going to move the file called `banana`, but this time I'm going to
specify a filename instead of a directory, just so you can see the difference:

   mv banana orange

Taking a look at our current directory, which is the default destination
if no directory is specified, we can see that there is no longer a file
named `banana`, but there's a new one named `orange`.  

    ls -l

In other words, in this case the file was simply renamed, which is a really
helpful side-effect of using the `mv` command.

So what would happen if I decided to move the `orange` file to a file that already
exists, like the `grape` file?  It would simply rename the `orange` file to `grape`,
so the contents of the original `grape` file would be replaced with whatever the
`orange` file contained:

    iMac:~ screencast$ mv orange grape

Listing the files, you can see that the `orange` file no longer exists,
because it was renamed to `grape`:

    iMac:~ screencast$ ls -l orange grape
    ls: orange: No such file or directory
    -rw-r--r--  1 screencast  staff  0 Nov  5 11:46 grape


## Removing files

<!--

   touch chuck charles chip

-->

Now that we've learned how to create files and manipulate them in a variety of
ways, sometimes it's useful to do a little housekeeping and clean up our mess.

Let's take a look at the files we've created first:

    ls -l

Then let's say I want to remove a file called "chuck", I simply use the
`rm` command followed by the name of that file:

    rm chuck

Alternatively, I can provide a list of files:

    rm charles chip orange

And we can see that those files were in fact removed:

    ls -l

The `rm` command also has the ability to remove directories.  I'll
create a directory with a few files just for this example:

    mdkir junk
    cd junk
    touch one two three
    cd ..
    ls -l junk

Now we can attempt to remove the directory:

    rm junk

But as you can see, this gives us an error message:

    rm: junk: is a directory

However, if I specify the "Recursive" option, like the following:

    rm -r junk

You can now see that the directory is indeed removed:

    ls -l


## Removing directories

Additionally, there is a command that is specific to removing directories.  That
command is `rmdir`.  So, let's setup a directory with some files again and try
it out:

    mdkir junk
    cd junk
    touch one two three
    cd ..

Here I'm simply creating a directory called `junk` which, contains three
different files: `one`, `two`, `three`.  Listing the contents of the
directory, you can see that it does in fact contain those files:

    ls -l junk

So, if I try to remove the directory using the `rmdir` command:

    rmdir junk

It will return an error, letting me know the directory is not empty:

    rmdir: junk: Directory not empty

So, let's see what happens if the directory is empty:

    rm junk/*

This time around, the command will work because the directory is indeed
empty:

    rmdir junk

In my view, using the `rmdir` command isn't that useful since the `rm`
command can not only remove files, but also directories, regardless of
whether they contain files and directories themselves, so I find that
I generally stick with using the `rm` command instead.


## Searching files

<!--

  ☺ cat TODO apollo collaboration
  Discuss the cat command
  Then cover head & tail
  Lastly, cover more & less
  "If you'll excuse me a minute, I'm going to have a cup of coffee."
  - broadcast from Apollo 11's LEM, "Eagle", to Johnson Space Center, Houston
    July 20, 1969, 7:27 P.M.
  Collaboration, n.:
          A literary partnership based on the false assumption that the
          other fellow can spell.

-->

Sometimes it's helpful to have the ability to search through files.  Maybe
you've been working on some markup in HTML or CSS for a website or writing some
code in a language such as Ruby or Javascript, and you need to search for
a pattern, such as CSS class name, a function or method name, or some string
that you need to change.  You might have many files that contain this string of
characters, so manually opening these files would be too cumbersome and
time-consuming to do in an effective way.  

In those situations, one command that would be helpful is `grep`.  First
let me show you a few files that I've setup that contain random items
within them.

`cat TODO`:

  Discuss the cat command
  Then cover head & tail
  Lastly, cover more & less

`cat apollo`:

  "If you'll excuse me a minute, I'm going to have a cup of coffee."
  - broadcast from Apollo 11's LEM, "Eagle", to Johnson Space Center, Houston
    July 20, 1969, 7:27 P.M.

`cat collaboration`:

  Collaboration, n.:
          A literary partnership based on the false assumption that the
          other fellow can spell.

First we have a TODO file that contains a few tasks that I wanted to
accomplish, then a file named "apollo" that contains a quote from that
mission, and finally a file called "collaboration" which contains
a sarcastic definition of the word.

So using those files, let me show you the basics of `grep`. First you type
the command, followed by any options then a pattern to search for,
followed by a file path or list of files that should be searched within,
like so:

    `grep` co TODO 

Here I'm searching for the pattern "c", "o" within the TODO file.

Alternatively, I could search through all 3 of those files with this command:

  `grep` co TODO apollo collaboration

As you can see, 2 of the files match the pattern provided.

Now let's try this same search passing the "i" option, which means to do
a case-insensitive search:

    `grep` -i the TODO apollo collaboration

Now we can see that all files match.

But what if we wanted to get the count of lines that match the pattern?  In that
case, we can just pass the "c" option:

    `grep` -ic co TODO apollo collaboration

Let's say we wanted to just see the files that matched the pattern, but
we weren't interested in the content?  In that case, we can just pass
the "l" option:

    `grep` -il co TODO apollo collaboration

From there we can see that only the file names are returned in the results.

The `grep` command also has an option for returning the lines in a file that do
not match a pattern.  So revisiting the contents of the "apollo" file:

    cat apollo  

    "If you'll excuse me a minute, I'm going to have a cup of coffee."
    - broadcast from Apollo 11's LEM, "Eagle", to Johnson Space Center, Houston
      July 20, 1969, 7:27 P.M.

Let's search again to see the results:

    `grep` co apollo

    "If you'll excuse me a minute, I'm going to have a cup of coffee."

And now let's see the lines that don't match the pattern, using the `-v`
option:

    `grep` -v co apollo

    - broadcast from Apollo 11's LEM, "Eagle", to Johnson Space Center, Houston
      July 20, 1969, 7:27 P.M.

And one of the most powerful features of `grep` is the ability to search
recursively through directories below our current directory for a specific
pattern by using the `r` option.

To prove that, I'm going to create a directory and move a couple of files to it:

    mkdir junk
    mv TODO apollo junk

And then I'm going to search again:

    `grep` -r co *

And you can see the results include files in the current directory and the
directory below that I just created.

Now, the `grep` command has quite a few variations, including `egrep`,
`fgrep`, `zgrep`, `zegrep` & `zfgrep`, as well as other commands that
provide similar functionality to `grep`, such as `ack` and finally `ag`
(known as "The Silver Searcher"), which is my personal favorite.

Considering the wide number of commands available for searching files,
as well as the variations of Regular Expression engines that come along
with those commands, it's difficult to learn every variation with great
detail.  In my own experience, I've tried to first learn the basics of
the commands I need, and then later experiment with other options or
commands.  Take small steps and build upon your existing knowledge.  To
begin that process, I highly encourage you to read the man pages for
`grep` and its counterpart commands to find the ones that work best for
you.  


## Finding files

Before we wrap up this chapter, I'd like to touch on a very important
command that becomes much more useful as your system grows.  Over time
you can accumulate more files and need to be able to quickly find them
or perform some general housekeeping, such as delete files that are
simply too large or unnecessary, or possibly change permissions for
a batch of files.

One of the best ways on UNIX to locate files is to use a command called
`find`.  The `find` command expects a path as its first argument so that
it knows where to start its search, followed by one or more options.

Let's look at a basic example:

    find ~ -size +10M

This says to find files starting from my home directory that have a size
greater than ten megabytes.

Here's another example:

    find /Applications -name "*.txt"

This says to find all files under the Applications directory that have
a name that contains any number of characters followed by a period and
the letters `txt`.

Here's another example using command execution on the result set:

    iMac:~ screencast$ find . -name "fruits*" -exec ls -l {} \;                                                                                                                        
    -rw-rw-rw-  1 screencast  staff  49 Jul 29 16:12 ./fruits
    -rw-rw-rw-  1 screencast  staff  6 Jul 29 14:24 ./fruits-2
    -rw-rw-rw-  1 screencast  staff  11 Jul 29 14:52 ./fruits-3
    -rw-rw-rw-  1 screencast  staff  27 Jul 29 16:39 ./fruits-4
    -rw-rw-rw-  1 screencast  staff  27 Jul 29 14:56 ./fruits-5
    -rw-rw-rw-  1 screencast  staff  10 Jul 29 16:16 ./fruits-6
    -rw-rw-rw-  1 screencast  staff  49 Jul 31 16:51 ./fruits.sorted

This command should start finding from the current directory, looking
for files or directories that start with the string "fruits", followed
by any number of characters, and then using the `-exec` switch, it
should execute the `ls -l` command against all files, which is specified
using the pair of curly braces.  The command should be ended with
a semicolon, but should be escaped so that shell expansion isn't
performed. 

Here's another example, but this time we'll specify that we only want it
to search for directories, indicated by specifying `d` for the `-type`
switch:

    iMac:~ screencast$ find . -type d -maxdepth 1
    .
    ./.cache
    ./.cups
    ./.dropbox
    ./.emacs.d
    ./.node-gyp
    ./.npm
    ./.ssh
    ./.Trash
    ./.vim
    ./bin
    ./Desktop
    ./Documents
    ./Downloads
    ./Dropbox
    ./Library
    ./Movies
    ./Music
    ./Pictures
    ./Public

Since the output would be extremely verbose, I've decided to also
specify a `-maxdepth` of `1`, just so it only searches through the
current directory and not in any directories beneath it.

To wrap up our examples of using `find`, you can also search for all
files matching a specific permission setting using the `-perm` switch:

    iMac:~ screencast$ find . -perm 644 -maxdepth 1
    ./.bash_profile
    ./.bashrc
    ./.DS_Store
    ./.gitconfig
    ./.irb_history
    ./.profile
    ./.vimrc
    ./df.out
    ./laffruits
    ./ls.out
    ./plist.out
    ./somefile

Again, we indicated a `-maxdepth` of `1` just to limit the output for
this example.

The `find` command is so powerful that it can not only search for file
sizes, name patterns, types, and permissions, but also owners, groups
and many, many other criteria.  Be sure to consult the man pages for the
various options available.


## Chapter Summary

So that wraps up this chapter on managing files and directories.  With these
commands you now have the ability to navigate the directory hierarchy as well as
find files and manipulate them with ease.
