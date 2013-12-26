# Understanding directory and file permissions

To fully understand how to manipulate files and directories, it is important to
remember that a file system can provide various levels of access to users
through a permissions-based system.  In this chapter, we'll cover the basics of
how to read and change permissions so that you can have a better understanding
of the file system, as well as how to manage who has access to files and
directories within it.


## Users and Groups

To start off, you need be aware that Users and Groups are central to UNIX
permissions.

Every account on a UNIX system has a unique user.  Using the `whoami` command,
I can see what login or username that is issuing commands.

    iMac:~ screencast$ whoami
    screencast

Every user belongs to at least one group.  Groups names are also unique.

So let's take a closer look at my user account, this time using the `id`
command:

    iMac:~ screencast$ id
    uid=503(screencast) gid=20(staff) groups=20(staff),12(everyone),61(localaccounts),100(_lpoperator),402(com.apple.sharepoint.group.1),403(com.apple.sharepoint.group.2)

We can also look at the output in a password file format, which gives us a lot
of other useful information about a user's account:

    iMac:~ screencast$ id -P
    screencast:********:503:20::0:0:Screencast:/Users/screencast:/bin/bash

The output is colon separated.  Starting from the left we have the login name or
username of the account, followed by their encrypted password, their user id
(or uid) and then their group id (or gid). The next field is unused.  Then we
have the time when the password was changed, the expiration time for the account
and the user's full name, their home directory and lastly we have their login
shell.


## Permission classes

Now that we know that Users and Groups are central to file and directory
permissions, it's important to know that this system contains 3 distinct
classes for permissions, which include the following:

    *  User

    *  Group

    *  Other

Examples of how each of these permission classes is used will be shown in the
**Commmon File Permissions** section later in this chapter.


**Side note:**

    The `User` class is also sometimes referred to as the `owner`
    of the file or directory.  The `Other` class is often referred to as
    "Everyone else".


## Permission notations

Permissions are applied to every file or directory to determine what a user or
group can do with it.  We can view those permissions by using the `ls` command
that we've seen previously:

    iMac:~ screencast$ ls -l
    total 4520
    drwx------+  5 screencast  staff      170 Nov  4 15:58 Desktop
    drwx------+  5 screencast  staff      170 Nov  4 12:54 Documents
    drwx------+ 36 screencast  staff     1224 Nov  2 18:37 Downloads
    drwx------@  5 screencast  staff      170 Oct 24 15:59 Dropbox
    drwx------@ 50 screencast  staff     1700 Oct 24 15:59 Library
    drwx------+  4 screencast  staff      136 Jun 11 17:30 Movies
    drwx------+  6 screencast  staff      204 Aug  3 10:16 Music
    drwx------+  6 screencast  staff      204 Jun 11 16:30 Pictures
    drwsrwsrwt+  5 screencast  staff      170 Aug 26 11:24 Public
    lrwxr-xr-x   1 screencast  staff       35 Oct 21 13:21 Screencasts -> /Volumes/Macintosh HD 2/Screencasts
    -rw-r--r--   1 screencast  staff        0 Nov  5 11:46 apple
    drwxr-xr-x   3 screencast  staff      102 Sep 11 20:11 bin
    -rw-r--r--   1 screencast  staff      467 Jul 31 19:19 df.out
    d--x--x--x   2 screencast  staff       68 Nov  5 18:01 goodbye
    -rw-r--r--   1 screencast  staff        0 Nov  5 11:46 grape
    -rw-r--r--   1 screencast  staff        0 Nov  5 10:28 laffruits
    -rw-r--r--   1 screencast  staff     1044 Jul 31 17:23 ls.out
    -rw-r--r--   1 screencast  staff  2296671 Aug 24 20:51 plist.out
    -rw-r--r--   1 screencast  staff       17 Jul 29 16:40 somefile

The output that is returned is displayed using "symbolic notation", which is
one of the 2 notation types that can be used when manipulating permissions.
The other notation type is called **Numeric notation** which I'll cover later
on, in the section of that same name.  Symbolic notation is easily recognizable
because it the default representation shown by using the `ls` command.


## Permission flags

Let's start with the left-most flag, which we've already discussed in a previous
chapter.

    b     Block special file.
    c     Character special file.
    d     Directory.
    l     Symbolic link.
    s     Socket link.
    p     FIFO.
    -     Regular file.

Now, the most common settings are "d" for a directory or a dash "-" for
a regular file.  But as you can see, this flag can also contain other settings
depending on if it's a block or character device, symbolic link, socket or named
pipe.

Moving on, we have remaining 9 flags.  

    rwxrwxrwx

Fortunately, these can be broken down by the 3 classes mentioned previously,
with the "User" class represented by the first 3 flags, the "Group" class shown in
the middle 3 flags, and the "Other" class shown in the remaining flags:

    rwx       rwx       rwx

    User      Group     Other


## Symbolic notation

So, what do these letters mean?

    r = `r` is the read flag.
    w = `w` is the write flag.
    x = `x` is the execute flag for a file. For a directory, you can "search" it.
    - = `-` means that this flag is not set.


## Common File permissions

So let's looks at a few common examples for **file** permissions.

### Example 1: readable, writable and executable by everyone

    rwxrwxrwx

    rwx       rwx       rwx

    User      Group     Other

Starting from the left, we can see that the first 3 flags are set, so the User
has read, write and execute permissions.  We can see that the same is true for
the 3 flags representing the Group, as well as the last 3 flags for the Other
class.

Here's a recap of what we just reviewed for Example 1:

    rwx       rwx       rwx

    User      Group     Other

    User has read, write and execute permissions
    Group has read, write and execute permissions
    Other has read, write and execute permissions


### Example 2: readable & executable by all, but writable only by owner

Let's look at a more common example that you might see for a file on your
system, like a compiled program that requires execute permissions.  For this
example, I'm going to look at the permissions of the `ls` command itself.  To do
this properly, we need to know the full path to the `ls` command, which we can
find by using the `whereis` command:

    iMac:~ screencast$ whereis ls
    /bin/ls
  
Now using the long format of the `ls` command, we can see what permissions it
has by passing the full path to the command:

    iMac:~ screencast$ ls -l /bin/ls
    -rwxr-xr-x  1 root  wheel  34736 Oct 23 13:44 /bin/ls

Let's just focus on the permissions part that is returned:

    -rwxr-xr-x

Starting from the left, we see the first flag is a dash, so it's not set,
meaning that it is a regular file and not a directory or some other device.

    rwxr-xr-x

    rwx       r-x       r-x

    User      Group     Other

Again, starting from the left, we see the first 3 flags that represent the User
class are set, so read, write and execute are accessible for the owner of the
file.  

This makes sense because a system utility like the `ls` command might
need to be overwritten with a newer version, as in the case of a system upgrade.

Moving on, we can see the middle 3 flags that represent the Group class show
that only the read and execute flags are set.  The same is true for the
remaining 3 flags that represent the Other class.  This is fairly standard
since neither the Group nor the Other class really need write permissions.
They only need to be able to read the command or execute the command.

Here's a recap of what we just reviewed:

    rwx       r-x       r-x

    User      Group     Other

    User has read, write and execute permissions
    Group has only read and execute permissions
    Other has only read and execute permissions


### Example 3: readable only by the owner, but no other permissions

    r--------

    r--       r--       r--

    User      Group     Other

In this example, we can see that the User only had **read** permission, but no
other permissions have been set.  Neither the Group nor the Other class has any
permissions whatsoever set.  This kind of permission might work well for a text
file or some other file where the owner is only interested in reading the
contents, but doesn't need to change it, and wants nobody else on the system to
have access to it.

Here's a recap of the permission settings what we just reviewed:

    r--       ---       ---

    User      Group     Other

    User has only read permission
    Group has no permissions
    Other has no permissions


## Common Directory permissions

Now let's looks at a few common examples for directory permissions.  From the
`HOME` directory, let's look at the output of `ls` using the **long** format:

    ls -l ~

    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public


### Device type attribute

Let's first take a look at the Library directory:

    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 Library

And diving down deeper, let's only focus on the permissions:

    drwx------@

Starting from the left, the first flag is a `d`, which means that this entry is
a directory.  

**Special note:** The first attribute specifies the file type.  Please
see **File types** section from the chapter on **Managing files and
directories** for more information.

We also see the first 3 flags that represent the User class are set, so read,
write and execute are accessible for the owner of this directory.  However, no
other permissions are set for the Group or Other classes, so that means that
this directory is private to the owner.

What this means is that other regular users on the system do not have access to
that directory.  To demonstrate this, I've logged into the system as another
account named `chip` and I'll attempt to change into the Library directory of
the `screencast` account:

    chip@iMac ~ 
    ☺ whoami
    chip
    ☺ cd ~screencast/Library
    cd: permission denied: /Users/screencast/Library

As you can see, it prevented the chip account from changing into that directory
and issued a "permission denied" error.  This is because chip is part of the
"Other" permission class, and that class doesn't have permissions to change
into that directory.  Remember the execute flag on a directory is
another way of saying that the directory allows access to that
permission class.


### The @ attribute

Revisiting the permissions for the Library directory for the `screencast` user,
you might have noticed something unusual:

    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 Library

If you're looking closely, you will notice a new character in the far right
position, which is an "at" sign `@`.  So, what does this represent? It means
that the directory has extended attributes.  These extended attributes can be
displayed using some special options of `ls`:

    ls -l@d ~/Library/

The first option is the "long" format that we've used numerous times before.
It's followed by the "at" symbol `@` flag that displays the extended
attributes.  The `d` flag displays the results without showing the contents of
the directory recursively:

    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 /Users/screencast/Library/
	    com.apple.FinderInfo	  32

Here we can see the extended attributes.  


### Extended attributes with xattr

Alternatively, the extended attributes can be viewed with the `xattr` command,
which shows the same information before, plus some additional details displayed
in hexadecimal:

    xattr -l ~/Library
    com.apple.FinderInfo:
    00000000  00 00 00 00 00 00 00 00 40 00 00 00 00 00 00 00  |........@.......|
    00000010  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00  |................|
    00000020

The full details of extended attributes will not be covered in this
`screencast`, but if you'd like to know more information, please consult the
man pages for the `ls` and `xattr` commands.

Let's revisit our `HOME` directory listing to find another example of directory
permissions:

    ls -l ~

    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public

Let's now take a look at the Public directory:

    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public

And diving down deeper, let's only focus on the permissions:

    drwxr-xr-x+

Starting from the left, the first flag is a `d`, which shows that this entry is
also a directory.  We also see the first 3 flags that represent the User class
are set, so read, write and execute are accessible for the owner of this
directory.  Read and execute permissions are set for both the Group and the
Other classes, which based on its name, makes sense for this directory because
we would want other users to be able to read it's contents and execute the `cd`
command to change into it.

To prove this, let's see if the `chip` account can list the contents of the
Public directory of the `screencast` account:

    chip@iMac ~ 
    ☺ ls -al ~screencast/Public
    total 0
    drwxr-xr-x+  4 screencast  staff  136 Jun 10 14:13 .
    drwxr-xr-x+ 19 screencast  staff  646 Jul 25 15:36 ..
    -rw-r--r--   1 screencast  staff    0 Jun 10 14:13 .localized
    drwx-wx-wx+  3 screencast  staff  102 Jun 10 14:13 Drop Box

And also, let's verify that we can change into that directory as well:

    chip@iMac ~ 
    ☺ cd ~screencast/Public
    chip@iMac ~screencast/Public 
    ☺ 

Moving on, let's revisit the permissions for the Public directory to show one
final setting: 
 
    drwxr-xr-x+

Looking closely, you'll see that the flag on the far right is a plus sign `+`.
This means that the directory has extended security information, which is
commonly found in ACL's, also known as Access Control Lists.

Since ACL's are a more advanced topic, I won't be covering them in this
tutorial, so please consult the the man pages for the `ls` and `chmod` commands
for more details.


### The chmod command

The command that can change file modes or Access Control Lists is the "change
mode" command, `chmod`, which I usually pronounce as "ch-mod".

In the examples that follow, we're going to start out using Symbolic Notation
with this command, so let's look at what each symbol represents:

For specifying the permission class, we have the following symbols:

    u user
    g group
    o other
    a all

For specifying the operation to perform, we have these symbols:

    + add permissions
    - remove permissions
    = set permissions

And finally, the symbols for the permissions themselves are:

    r = `r` is the read flag.
    w = `w` is the write flag.
    x = `x` is the execute flag for a file. For a directory, you can "search" it.


## Example of read permission for owner of file

Taking a look at a file called `hello`, I can see that it currently has
an `echo` statement within it:

    iMac:~ screencast$ cat hello 
    echo "Hello world"

I'll remove the `read` permissions for the User class of the file and
verify that those permissions have been changed properly:

    iMac:~ screencast$ chmod u-r hello
    iMac:~ screencast$ ls -l hello 
    --wxr--r--  1 screencast  staff  19 Jul 26 15:30 hello

Attempting to view the contents of the file again produces an error,
which is expected because I don't have `read` permissions any longer:

    iMac:~ screencast$ cat hello
    cat: hello: Permission denied

I'll then add back the `read` permissions and verify that I can view the
file again:

    iMac:~ screencast$ chmod u+r hello
    iMac:~ screencast$ ls -l hello
    -rwxr--r--  1 screencast  staff  19 Jul 26 15:30 hello
    iMac:~ screencast$ cat hello
    echo "Hello world"

In both of these examples, I followed a format of specifying the symbol
for the permissions class (`u` for the User class), followed by the
operation to be performed (`+` for adding permissions or `-` for
removing permissions), followed by the permissions themselves (`r` for
`read` permission).


## Example of write permission for owner of file

Using the same file, I'll now remove the `write` permission for the
`User` class:

    iMac:~ screencast$ chmod u-w hello

Attempting to write to the file produces an error:

    ☺ echo 'abc' > hello
    permission denied: hello

I can add the write permission back and attempt to write to the file
again:

    iMac:~ screencast$ chmod u+w hello
    iMac:~ screencast$ echo 'abc' > hello 

Viewing the file one last time shows that it is now different because it
was writable by the owner:

    iMac:~ screencast$ cat hello
    abc


## Example of execute permission for owner of file

In this example, I have a file that will echo a string:

    iMac:~ screencast$ cat hello
    echo "abc"

Attempting to execute the file will produce an error:

    iMac:~ screencast$ ./hello
    -bash: ./hello: Permission denied

This is because the owner doesn't have execute permissions:

    iMac:~ screencast$ ls -l hello
    -rw-r--r--  1 screencast  staff  13 Jul 26 15:06 hello

Let's add those permissions:

    iMac:~ screencast$ chmod u+x hello

And then try to execute the file again:

    iMac:~ screencast$ ./hello 
    abc

This time it echoed the string, which is due to it having the proper
permisions.


## Example of read permission for owner of directory

In this example, I'm going to create a directory which contains a couple
of empty files:

    iMac:~ screencast$ mkdir test
    iMac:~ screencast$ touch test/TODO
    iMac:~ screencast$ touch test/TODO-2
    iMac:~ screencast$ ls -l test
    total 0
    -rw-r--r--  1 screencast  staff  0 Jul 26 15:14 TODO
    -rw-r--r--  1 screencast  staff  0 Jul 26 15:17 TODO-2

I'm now going to remove the `read` permissions for the User class from
the directory:

    iMac:~ screencast$ chmod u-r test

Attempting to list the files in that directory should cause an error
because I no longer have `read` permissions:

    iMac:~ screencast$ ls -l test
    ls: test: Permission denied

However, because I have execute permissions for the directory I can use
`cd` to change into it:

    iMac:~ screencast$ cd test

But, I still cannot list or read the contents of the directory:

    iMac:test screencast$ ls -l
    ls: .: Permission denied

Changing the permissions back to being readable for the owner of the
directory, I can now list the contents:

    iMac:test screencast$ cd ..
    iMac:~ screencast$ chmod u+r test
    iMac:~ screencast$ ls -al test
    total 0
    drwxr-xr-x   4 screencast  staff  136 Jul 26 15:17 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:14 TODO
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:17 TODO-2


## Example of write permission for owner of directory

In this example, we'll first view the contents of the directory and then
remove the write permission for the owner of it:

    iMac:~ screencast$ ls -al test
    total 0
    drwxr-xr-x   4 screencast  staff  136 Jul 26 15:17 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:14 TODO
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:17 TODO-2
    iMac:~ screencast$ chmod u-w test
    iMac:~ screencast$ ls -al test
    total 0
    dr-xr-xr-x   4 screencast  staff  136 Jul 26 15:17 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:14 TODO
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:17 TODO-2

And we can see that the permissions were applied properly.  Let's
attempt to write to the directory by adding another file:

    iMac:~ screencast$ touch test/TODO-3
    touch: test/TODO-3: Permission denied

This produces an error, and we can verify that the file was not added:

    iMac:~ screencast$ ls -al test
    total 0
    dr-xr-xr-x   4 screencast  staff  136 Jul 26 15:17 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:14 TODO
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:17 TODO-2

Let's add the write permission for the owner and then attempt to add the
file again:

    iMac:~ screencast$ chmod u+w test
    iMac:~ screencast$ touch test/TODO-3
    iMac:~ screencast$ ls -al test
    total 0
    drwxr-xr-x   5 screencast  staff  170 Jul 26 15:41 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:14 TODO
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:17 TODO-2
    -rw-r--r--   1 screencast  staff    0 Jul 26 15:41 TODO-3

This time the file was added properly to directory because the owner had
write permissions to do so.


## Example of execute permission for owner of a directory

In this example I will remove the execute permissions for the owner of
the directory:

    iMac:~ screencast$ ls -al test
    total 0
    drwxr-xr-x   2 screencast  staff   68 Jul 26 15:14 .
    drwxr-xr-x+ 25 screencast  staff  850 Jul 26 15:14 ..
    iMac:~ screencast$ chmod u-x test
    iMac:~ screencast$ ls -l 
    total 8
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 42 screencast  staff  1428 Jul 20 15:09 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public
    -rwxr--r--   1 screencast  staff    13 Jul 26 15:06 hello
    drw-r-xr-x   3 screencast  staff   102 Jul 26 15:14 test

Now that they're removed, we can try to list the contents of the
directory:

    iMac:~ screencast$ ls -l test
    ls: Permission denied

However, we received an error message because even though the owner had
read permissions, the `ls` command could not change into the directory
because there were no execute permissions set for the owner.


## Combinations of symbol notation

Looking at symbol notation a bit closer, we can pass much more complex
permission combinations provided we follow the proper syntax.

Let's revisit the permissions for the `hello` file:

    iMac:~ screencast$ ls -l hello
    -r-xrwxrwx  1 screencast  staff  19 Jul 26 15:30 hello

And this time I want to add `write` permission for the owner of the
file, but I want to remove that permission for both the Group and Other
classes.  This can be done by comma-delimiting the permission classes:

    iMac:~ screencast$ chmod u+w,g-w,o-w hello

Let's verify how the permissions were changed:

    iMac:~ screencast$ ls -l hello
    -rwxr-xr-x  1 screencast  staff  19 Jul 26 15:30 hello

Now let's set all of the classes to have no permissions whatsoever:

    iMac:~ screencast$ chmod ugo= hello

<!-- Try chmod a= instead? -->

    iMac:~ screencast$ ls -l hello
    ----------  1 screencast  staff  19 Jul 26 15:30 hello

We can see that the permissions were indeed removed, so let's add some
back, but this time we can combine the Group and Other classes since we
want to set `read` and `execute` permissions for both:

    iMac:~ screencast$ chmod u+rwx,go=rx hello

And then we can verify that those permissions were changed properly:

    iMac:~ screencast$ ls -l hello
    -rwxr-xr-x  1 screencast  staff  19 Jul 26 15:30 hello


## Execute flag is special

The execute is flag is a special one because it can be set to a wide
variety of symbols, which we'll discuss here.


## X flag

First is the `X` flag, which is the uppercase version of the execute
flag, and it has a special meaning.


# Effect of X flag on a file

If a file is already executable, adding the `X` flag to the permissions
makes it executable by all permission classes.

First we'll reset the file to have no permissions:

    iMac:~ screencast$ chmod a= hello

Next we'll add execute permissions for the group:

    iMac:~ screencast$ chmod g=x hello
    iMac:~ screencast$ ls -l hello
    ------x---  1 screencast  staff  19 Jul 26 15:30 hello

And finally, we'll add read and write permissions for the owner, but
this time we'll specify the `X` flag as well:

    iMac:~ screencast$ chmod u=rw,+X hello

Since the file already originally contained an execute permission for
the group, it should now have all permission classes set to executable:

    iMac:~ screencast$ ls -l hello
    -rwx--x--x  1 screencast  staff  19 Jul 26 15:30 hello

However, it only has an effect when used in the additive case.  Using it
to remove the permission has no effect:

    iMac:~ screencast$ chmod -X hello
    iMac:~ screencast$ ls -l hello
    -rwx--x--x  1 screencast  staff  19 Jul 26 15:30 hello


# Effect of X flag on a directory

To see the effect of the `X` flag on a directory, I'll create one first:

    iMac:~ screencast$ mkdir goodbye
    iMac:~ screencast$ ls -al goodbye
    total 0
    drwxr-xr-x   2 screencast  staff   68 Jul 27 11:08 .
    drwxr-xr-x+ 27 screencast  staff  918 Jul 27 11:08 ..

Next I'll remove the permissions for it and verify they were changed:

    iMac:~ screencast$ chmod a= goodbye
    iMac:~ screencast$ ls -l 
    total 8
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public
    d---------   2 screencast  staff    68 Jul 27 11:08 goodbye
    -rwx--x--x   1 screencast  staff    19 Jul 26 15:30 hello
    drwxr-xr-x   5 screencast  staff   170 Jul 26 15:41 test

And finally I'll apply the `X` flag in the additive mode and verify that
all classes are now executable:

    iMac:~ screencast$ chmod +X goodbye
    iMac:~ screencast$ ls -l 
    total 8
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-x+  4 screencast  staff   136 Jun 10 14:13 Public
    d--x--x--x   2 screencast  staff    68 Jul 27 11:08 goodbye
    -rwx--x--x   1 screencast  staff    19 Jul 26 15:30 hello
    drwxr-xr-x   5 screencast  staff   170 Jul 26 15:41 test

When not used in the additive mode, the `X` flag is ignored:

    iMac:~ screencast$ chmod -X goodbye/
    iMac:~ screencast$ ls -l
    total 4520
    drwx------+  5 screencast  staff      170 Nov  4 15:58 Desktop
    drwx------+  5 screencast  staff      170 Nov  4 12:54 Documents
    drwx------+ 36 screencast  staff     1224 Nov  2 18:37 Downloads
    drwx------@  5 screencast  staff      170 Oct 24 15:59 Dropbox
    drwx------@ 50 screencast  staff     1700 Oct 24 15:59 Library
    drwx------+  4 screencast  staff      136 Jun 11 17:30 Movies
    drwx------+  6 screencast  staff      204 Aug  3 10:16 Music
    drwx------+  6 screencast  staff      204 Jun 11 16:30 Pictures
    drwsrwsrwt+  5 screencast  staff      170 Aug 26 11:24 Public
    lrwxr-xr-x   1 screencast  staff       35 Oct 21 13:21 Screencasts -> /Volumes/Macintosh HD 2/Screencasts
    -rw-r--r--   1 screencast  staff        0 Nov  5 11:46 apple
    drwxr-xr-x   3 screencast  staff      102 Sep 11 20:11 bin
    -rw-r--r--   1 screencast  staff      467 Jul 31 19:19 df.out
    d--x--x--x   2 screencast  staff       68 Nov  5 18:01 goodbye
    -rw-r--r--   1 screencast  staff        0 Nov  5 11:46 grape
    -rw-r--r--   1 screencast  staff        0 Nov  5 10:28 laffruits
    -rw-r--r--   1 screencast  staff     1044 Jul 31 17:23 ls.out
    -rw-r--r--   1 screencast  staff  2296671 Aug 24 20:51 plist.out
    -rw-r--r--   1 screencast  staff       17 Jul 29 16:40 somefile


## setuid

The setuid concept, which is a shorthand way of saying, "set user id upon
execution", is a flag used to determine what user a file will be run as upon
execution.  This idea will make more sense when we cover UNIX processes later
on, so for now let's just focus on how we can identify this setting and change
it to suit our needs.

I'll first reset the `hello` file to have no permissions:

    iMac:~ screencast$ chmod a= hello
    iMac:~ screencast$ ls -l hello
    ----------  1 screencast  staff  13 Jul 27 11:54 hello

Next I'll add the `setuid` bit for the owner:

    iMac:~ screencast$ chmod u+s hello
    iMac:~ screencast$ ls -l hello
    ---S------  1 screencast  staff  13 Jul 27 11:54 hello

We see the flag is represented by an uppercase `S` because it's set for the
file owner, but when we add execute permissions, the flag is changed to a
lowercase `s`:

    iMac:~ screencast$ chmod u+x hello
    iMac:~ screencast$ ls -l hello
    ---s------  1 screencast  staff  13 Jul 27 11:54 hello

Removing the `setuid` flag from the owner will uncover that the file now only
has execute permissions set for the owner:

    iMac:~ screencast$ chmod u-s hello
    iMac:~ screencast$ ls -l hello
    ---x------  1 screencast  staff  13 Jul 27 11:54 hello

Let's recap the `setuid` notation:

    s     If in the owner permissions, the file is executable and set-user-ID mode
    is set.  

    S     If in the owner permissions, the file is not executable and set- user-ID
    mode is set.  


## setgid

The setgid concept, which is a shorthand way of saying, "set group id upon
execution", is a flag used to determine what group a file will be run as upon
execution.  

First, we'll reset the file to have no permissions and then add the `setgid`
permission:

    iMac:~ screencast$ chmod a= hello
    iMac:~ screencast$ chmod g+s hello
    iMac:~ screencast$ ls -l hello
    ------S---  1 screencast  staff  13 Jul 27 11:54 hello

We see the flag is represented by an uppercase `S` because it's set for the
file group, but when we add execute permissions, the flag is changed to a
lowercase `s`:

    iMac:~ screencast$ chmod g+x hello
    iMac:~ screencast$ ls -l hello
    ------s---  1 screencast  staff  13 Jul 27 11:54 hello

Removing the `setgid` flag from the group will uncover that the file now only
has execute permissions set for the group:

    iMac:~ screencast$ chmod g-s hello
    iMac:~ screencast$ ls -l hello
    ------x---  1 screencast  staff  13 Jul 27 11:54 hello

Let's recap the `setgid` notation:

    s     If in the group permissions, the file is executable and setgroup-ID
    mode is set.

    S     If in the group permissions, the file is not executable and
    set-group-ID mode is set.


## Sticky bit

The sticky bit is a technique that is commonly used on shared directories, or
directories containing temporary files, such as the `/private/tmp` directory on
the Mac:

    iMac:~ screencast$ ls -al /private
    total 0
    drwxr-xr-x@   7 root  wheel   238 Jul 27 12:49 .
    drwxr-xr-x   32 root  wheel  1156 Jul 17 14:57 ..
    drwxr-xr-x  111 root  wheel  3774 Jul 22 16:20 etc
    drwxr-xr-x    2 root  wheel    68 Jun 20  2012 tftpboot
    drwxrwxrwt   26 root  wheel   884 Jul 27 17:22 tmp
    drwxr-xr-x   25 root  wheel   850 Jun 19 18:50 var

This means that the directory itself is append-only, meaning that
files can only be added, not deleted.

Let's take a look at a few examples of how the sticky bit is used in practice.


### User has write permissions on the directory and owns the file

To be clear, files actually can be deleted when using the `sticky` bit,
provided that the user has write permission for the directory and if the file
is being deleted either by its owner or by the superuser.

To prove this, I'll create a file and check that I have write permissions:

    iMac:~ screencast$ touch /private/tmp/testfile
    iMac:~ screencast$ ls -l /private/tmp/testfile 

Now I'll try to remove the file, which should return without error:

    -rw-r--r--  1 screencast  wheel  0 Jul 27 17:25 /private/tmp/testfile
    iMac:~ screencast$ rm /private/tmp/testfile 


### User has write permissions, but doesn't own the file

In this example, we can see a file under the `/private/tmp` directory which is
not owned by the `screencast` account:

    iMac:~ screencast$ ls -l /private/tmp/mysql.sock 
    srwxrwxrwx  1 chip  wheel  0 Jul 17 14:58 /private/tmp/mysql.sock

If we attempt to remove that file, it should result in an error since the
directory has the `sticky` bit set:

    iMac:~ screencast$ rm /private/tmp/mysql.sock 
    rm: /private/tmp/mysql.sock: Permission denied


### Directory does not have execute (search) permissions

In this example, we'll remove the execute permission from the Other class:

    iMac:~ screencast$ sudo chmod o-x /private/tmp/

Without the execute permissions, the `sticky` bit is set, but is indicated with
the `T` in uppercase:

    iMac:~ screencast$ ls -l /private/
    total 0
    drwxr-xr-x  111 root  wheel  3774 Jul 22 16:20 etc
    drwxr-xr-x    2 root  wheel    68 Jun 20  2012 tftpboot
    drwxrwxrwT   26 root  wheel   884 Jul 27 17:30 tmp
    drwxr-xr-x   25 root  wheel   850 Jun 19 18:50 var

Adding the execute permission back, we can now see that the `sticky` bit is
reset as a `t` in lowercase form:

    iMac:~ screencast$ sudo chmod o+x /private/tmp/
    iMac:~ screencast$ ls -l /private/
    total 0
    drwxr-xr-x  111 root  wheel  3774 Jul 22 16:20 etc
    drwxr-xr-x    2 root  wheel    68 Jun 20  2012 tftpboot
    drwxrwxrwt   26 root  wheel   884 Jul 27 17:30 tmp
    drwxr-xr-x   25 root  wheel   850 Jun 19 18:50 var

Let's recap the notation for the `sticky` bit:

    These next two apply only to the third character in the last group (other per-
    missions).

    T     The sticky bit is set (mode 1000), but not execute or search permission

    t     The sticky bit is set (mode 1000), and is searchable or executable


## Numeric notation 

Now that we've covered the basics of the `chmod` command using symbolic
notation, it's time to look at an alternative format for manipulating file and
directory permissions.  That alternative is called **Numeric notation** and it
provides the ability to change permissions in a much less verbose manner than
symbolic notation.

So let's revisit an example of symbolic notation:

    chmod u=rwx,go=rx file

This explicitly says that the User class should be set to
read, write and execute permissions, and the Group and Other class
should be set to only read and execute permissions.

Using numeric notation, this can be done just as easily like this:

    chmod 755 file

So how does `u=rwx,go=rx` get converted to `755`?


## Binary numbers

To understand it fully, we need to provide a brief explanation of the
Binary number system.  

The Binary number system is a base 2 system, which basically means that
every number is a power of 2, and also that we only have 2 choices: 0 and 1.

Without getting too complicated, let's look at an example first.  


## Converting from decimal to binary

What I'd like to do is write the number 13 in binary.

So let's take a look at the Powers of 2:

    Powers of 2:    2^3     2^2     2^1     2^0

Starting from the right, we have 2 to the zero-th power, which is 1.
This is true in any numbering system.  Any number to the zero-th power
is always 1.

Moving one place to the left, we have 2 to the 1st power, which is
basically itself, or 2.

Moving another place to the left, we have 2 to the 2nd power, which is:

    2 * 2 = 4

Again moving to the left, we have 2 to the 3rd power, which is:

    2 * 2 * 2 = 8

Let's recap what we just discussed:

    Powers of 2:     2^3     2^2     2^1     2^0

                      8       4       2       1

So what we need to ask ourselves is, "How many 8's does it take to add up to
the number 13?".  It only takes 1, so let's set that column to 1.  If we
subtract 8 from from 13, we have only 5 left.   Moving right, we can look at
the next column and ask, "How many 4's can add up to the 5 we have remaining?"
The answer is 1, so we set that column.  That leaves us with only 1 remaining.
Again, moving right we can ask, "How many 2's add up to 1?"  The answer is none
because 2 is bigger than 1, so we don't set that place.  And finally, since
there's only 1 left and that's the last option, we set it.

Let's recap what we just did again visually:

    How do we represent the number 13 in binary?

    Powers of 2:     2^3     2^2     2^1     2^0

                      8       4       2       1

                      1       1       0       1

    Decimal: 13 
    Binary:  1101


## Converting from binary to decimal

So now that we've shown how to convert decimal to binary, let's try it
the other way around.  I'll go through a few examples, all of which you'll
see crop up many times over when setting permissions.

### Example in binary:  111

    Powers of 2:     2^2     2^1     2^0

                      4       2       1

    Binary:           1       1       1

    Decimal: 4 + 2 + 1 = 7 

Revisiting our Powers of 2, we see that the place containing 4 is set,
as well as the place containing 2 and finally 1.  Adding them up is as
simple as:

    4 + 2 + 1 = 7

If we look at it from a permissions perspective, 7 is the same as having
the read, write and execute bits set:

    Permissions:      r       w       x
    Binary:           1       1       1


### Example in binary:  101

    Powers of 2:     2^2     2^1     2^0

                      4       2       1

    Binary:           1       0       1

    Decimal: 4 + 0 + 1 = 5

Revisiting our Powers of 2, we see that the place containing 4 is set.
But the place containing 2 is not.  Finally the place containing 1 is
also set.  Adding them up is as simple as:

    4 + 0 + 1 = 5 

If we look at it from a permissions perspective, 5 is the same as having
the read and execute bits set:

    Permissions:      r       w       x 
    Binary:           1       0       1


### Example in binary:  100

    Powers of 2:     2^2     2^1     2^0

                      4       2       1

    Binary:           1       0       0

    Decimal: 4 + 0 + 0 = 4

Revisiting our Powers of 2, we see that the place containing 4 is set.
But the place containing 2 is not.  Neither is the place containing 1.
Adding them up is as simple as:

    4 + 0 + 0 = 4 

If we look at it from a permissions perspective, 4 is the same as having
the read bit set:

    Permissions:      r       w       x 
    Binary:           1       0       0


### An final example permission

So, now that we can convert from binary to decimal, let's take a look
at an example to really drive things home. Let's say we have the
following permission settings: 

    -rwxr-x---

Let's separate these out into their respective permission classes:

    User   Group   Other 
    rwx    r-x     ---

We'll take this one class at a time.  Starting with the User class, we
see that the read, write and execute bits are set.

    User   Group   Other 
    rwx    r-x     ---

    111

Moving on the Group class, we see that only the read and execute bits
are set:

    User   Group   Other 
    rwx    r-x     ---

    111    101

And finally, the Other class has none of the bits set:

    User   Group   Other 
    rwx    r-x     ---

    111    101     000

Converting these binary numbers to decimal, we now have 7 for the User
class, 5 for the Group and 0 for Other.

    User   Group   Other 
    rwx    r-x     ---

    111    101     000

     7      5       0

In other words, the numerical representation of these permissions using
`chmod` would be this:

    chmod 750 file


## Special flags using numerical notation

We've already covered the basics of setting permissions, but there still are a
few **special flags** that need to be addressed.  Those are `setuid`, `setgid`
and the `sticky` bit, which were covered previously in the section on symbolic
notation.  These flags will occupy another bit position, to the left of the
normal permission flags:

                User    Group   Other

     Special    User    Group   Other

So as an example, if we had read and execute permissions set for all
permission classes, and write permission set for only the User class,
we'd have something like this:

    chmod 755 file

If we wanted to set a special flag, we would place another number to the 
left of the permissions:

    chmod 4755 file

That command is basically the same as what was shown previously, but
with the `setuid` flag set, indicated with the number 4.  To understand
what numbers are associated with the various special flags, let's go
through a few examples.

Let's revisit the powers of 2 again:

Powers of 2:     2^2     2^1     2^0

                  4       2       1

This time I'm going to show each of the permission classes under the
numbers, just to illustrate a point:

Powers of 2:     2^2     2^1     2^0

                  4       2       1

                User    Group   Other

Then I'm going to show how each of the special flags are assigned. 

Powers of 2:     2^2     2^1     2^0

                  4       2       1

                User    Group   Other

                setuid  setgid  sticky

Lastly, I'm going to copy down the decimal numbers, just for
readability:

Powers of 2:     2^2     2^1     2^0

                  4       2       1

                User    Group   Other

                setuid  setgid  sticky

                  4       2       1

The `setuid` flag is assigned to the User class, which has a value of 4.
This makes sense because its purpose is to set a flag for the User's
process id.

Moving to the right, we see that the `setgid` flag is assigned to the
Group class, which has a value of 2.  This makes sense because its
purpose is to set a flag for the Group's process id.

Lastly, we see on the far right that the sticky flag is assigned to the
Other class, which has a value of 1.  This makes sense because its
purpose is to set a flag for the directory, which should be applied to
everyone.

To recap, let's take a look at each flag and it's corresponding value:

                setuid  setgid  sticky

                  4       2       1

Remembering these numbers will come in handy in the `chmod` examples
that follow.


## Setuid example using numerical notation

Let's start out with a file called **myfile** that has no permissions
set:

    iMac:~ screencast$ touch myfile
    iMac:~ screencast$ chmod a= myfile
    iMac:~ screencast$ ls -l myfile 
    ----------  1 screencast  staff  0 Jul 28 12:24 myfile

Applying symbolic notation, where we have read, write, execute and
`setuid` set for the User class and then read and execute flags set for
the Group and Other classes, it would look like this:

    iMac:~ screencast$ chmod u=rwxs,go=rx myfile 
    iMac:~ screencast$ ls -l myfile 
    -rwsr-xr-x  1 screencast  staff  0 Jul 28 12:24 myfile

However, that's far too verbose.  Let's try this another way.

I'm going to reset the flags so that we can start with a clean slate:

    iMac:~ screencast$ chmod a= myfile
    iMac:~ screencast$ ls -l myfile 
    ----------  1 screencast  staff  0 Jul 28 12:24 myfile

Using numerical notation, we can simply do this:

    iMac:~ screencast$ chmod 4755 myfile
    iMac:~ screencast$ ls -l myfile 
    -rwsr-xr-x  1 screencast  staff  0 Jul 28 12:24 myfile


## Setgid example using numerical notation

Now let's look at how to set the `setgid` flag.  Again, let's clear out the
permissions:

    iMac:~ screencast$ chmod a= myfile

Now we'll apply the permissions using the verbose symbolic notation:

    iMac:~ screencast$ chmod u=rwx,g=rxs,o=rx myfile
    iMac:~ screencast$ ls -l myfile 
    -rwxr-sr-x  1 screencast  staff  0 Jul 28 12:24 myfile

That works fine, but I'd rather use numerical notation because it's
easier and shorter if you understand the rules.  I'll clear out the
permission again:

    iMac:~ screencast$ chmod a= myfile
    iMac:~ screencast$ ls -l myfile 
    ----------  1 screencast  staff  0 Jul 28 12:24 myfile

And then I'll set the `setgid` flag using numerical notation:

    iMac:~ screencast$ chmod 2755 myfile
    iMac:~ screencast$ ls -l myfile 
    -rwxr-sr-x  1 screencast  staff  0 Jul 28 12:24 myfile


## Sticky bit example using numerical notation

And now let's look at our example for setting the `sticky` bit.  Since
this special flag doesn't make sense on a file, I'll be using my
`Public` directory for my account. Again, let's clear out the
permissions:

    iMac:~ screencast$ chmod a= Public/
    iMac:~ screencast$ ls -l
    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    d---------+  4 screencast  staff   136 Jun 10 14:13 Public

We can see that it has no permissions, so let's set them using symbolic
notation:

    iMac:~ screencast$ chmod u=rwx,go=rx,+t Public/

Again, we have read and execute permissions on all classes, and write
permission for the user, but this time we're also setting the `sticky` bit
for the directory, as shown with the "plus t" `+t` notation. 

    iMac:~ screencast$ ls -l
    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-t+  4 screencast  staff   136 Jun 10 14:13 Public

And as you can see, the directory now has those permissions.  Now let's
clear out the settings again:

    iMac:~ screencast$ chmod a= Public/

And now we'll apply the same permissions, but this time using Numerical
notation:

    iMac:~ screencast$ chmod 1755 Public/
    iMac:~ screencast$ ls -l
    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwxr-xr-t+  4 screencast  staff   136 Jun 10 14:13 Public

Again, we have the same result as before, but this time it was
accomplished in a much simpler fashion using numerical notation.


## Wrapping up the special flags

Keep in mind that any combination of flags can be used, so you'll just
have to add up the numbers to get the right setting.  Let's revisit the
special flag assignments:

                setuid  setgid  sticky

                  4       2       1

So, if we wanted to have read, write and execute permissions for the
owner of a file, but also have `setuid` and `setgid` set, we'd first add the
values for those special flags: 4 + 2 = 6.  Then we'd apply the flags
for the owner, like so:

    iMac:~ screencast$ touch myfile
    iMac:~ screencast$ chmod a= myfile
    iMac:~ screencast$ ls -l myfile
    ----------  1 screencast  staff  0 Jul 28 13:06 myfile
    iMac:~ screencast$ chmod 6700 myfile
    iMac:~ screencast$ ls -l myfile
    -rws--S---  1 screencast  staff  0 Jul 28 13:06 myfile
    iMac:~ screencast$ 

If we wanted all special flags on a directory and have full permissions,
we'd do something like this:

    iMac:~ screencast$ chmod a= Public/
    iMac:~ screencast$ ls -l
    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    d---------+  4 screencast  staff   136 Jun 10 14:13 Public
    iMac:~ screencast$ chmod 7777 Public/
    iMac:~ screencast$ ls -l
    total 0
    drwx------+  4 screencast  staff   136 Jul 22 14:46 Desktop
    drwx------+  9 screencast  staff   306 Jul 16 19:54 Documents
    drwx------+  8 screencast  staff   272 Jul 25 16:36 Downloads
    drwx------@ 43 screencast  staff  1462 Jul 27 10:54 Library
    drwx------+  4 screencast  staff   136 Jun 11 17:30 Movies
    drwx------+  5 screencast  staff   170 Jun 15 17:00 Music
    drwx------+  6 screencast  staff   204 Jun 11 16:30 Pictures
    drwsrwsrwt+  4 screencast  staff   136 Jun 10 14:13 Public


So that covers the special flags.  If you're still having trouble, please go
back and look at the examples and follow along in your Terminal.  Experiment
with various settings on your own to get comfortable.  I would suggest that you
master symbolic notation first, just to get comfortable.  Before long you'll
probably find that Numerical notation is much easier and faster, but if not you
always have more than one way to accomplish setting permissions.


## Permissions wrap up

Before we wrap up this chapter on file and directory permissions, I want
to point out a couple of commands there are central to this system.

The first of those commands is the `chown` command:

    
    The `chown` command gives you the ability to change the owner and
    group of a file or directory.  


The second of those commands is the `chgrp` command:

    The `chgrp` is similar to `chown`, but it is limited to only
    changing the group of a file or directory.

I won't be showing examples of these commands because they are more commonly
used from the superuser account.  The superuser account is often referred to as
the "root" account and is reserved for handling systems administration on any
UNIX system.  Since that's a much more advanced concept, I'm going to reserve
that for an entirely separate course than the one I'm teaching here.

So that covers file and directory permissions.  I encourage you to experiment
on your own so that you can get comfortable with managing your own files.  Just
remember that it sometimes takes a lot of practice, so don't get discouraged if
it isn't easy in the early stages.  Just keep practicing and eventually it will
become second nature.

