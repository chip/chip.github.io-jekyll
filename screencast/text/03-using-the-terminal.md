# Getting started using the Terminal

In this section, we'll need to select software that gives us the ability to type
in shell commands.  Before we even get into the various concepts and
commands that UNIX offers us, we need an application that gives us a command
line, which is essentially an interface to a shell.

This kind of software is often referred to as a terminal emulation program, or
more commonly, a terminal.

Throughout the history of UNIX there have been many different terminal
applications made, such as xterm, which has been around for many years, and also
iTerm2 which I've found to be quite popular in the Ruby programming community.

As a UNIX user, you have many terminal choices available to you, but for the
purposes of this tutorial, I'll be using the default Terminal application that
comes shipped with OS X, so there's nothing extra that you'll need to install to
get started.


## Accessing the Terminal

With our terminal choice out of the way, let me demonstrate some of the more
common ways for accessing it.


### The Finder

If you're the kind of user that prefers using a mouse or trackpad, I would
suggest that you use the Finder to access the Terminal.  By default, the Finder
is located on the Dock, which should be at the bottom of your screen.  As you
can see, the Finder is represented as a blue smiley face icon on the far left of
the Dock.

To access the Terminal from here, you can click the Finder, which should load
a window containing a number of files and folders located under your account.

As a side note, whenever you hear the term "folder", remember that it is
synonymous with the term "directory", which is how it's more commonly referred
to in UNIX circles.  Regardless of which term you use, a folder - or directory
- is simply a container for other files and directories.

With the window now open, I'll first maximize it to make it easier to find, and
then I'll scroll down until I find the Utilities folder.  I'll then double-click
on the Utilities folder, and then scroll down to find the Terminal application.
And finally, Double-clicking on the icon will launch the Terminal.

Personally, I think that's too many steps to access the Terminal, so let's look
at a few other ways to quickly access it.


### The Menu bar

Apart from clicking on the Finder icon located on your Dock, another
option for accessing the Terminal is by using the Finder menu at the top
of your screen.  From there you can select the Go menu and then click on
the Utilities menu option.  From within the Utilities folder you can
click on the Terminal icon located near the bottom of the list.


### The Utilities keyboard shortcut

If you are accustomed to using keyboard shortcuts, you can alternatively access
the Utilities menu by simply pressing the shortcut:

  Shift Cmd-U


### Creating a Desktop shortcut

The easiest thing to do now that we've found the Terminal is to simply
create a Desktop shortcut from here.  First, be sure you have the
Terminal selected.  Then, use the mouse to drag the icon over to your
Desktop.  Now that you have created the Desktop shortcut, you can simply
double-click on the Terminal whenever you need it.


### Access it using Spotlight

Out of all of these different options, my personal favorite for quickly
accessing the Terminal, or any Mac application for that matter, is by using the
Spotlight feature, which can be accessed by pressing a keyboard shortcut:

  Cmd-Space

Once Spotlight is launched, just start typing the word "Terminal" into the text
field and Spotlight will immediately start populating the list with items that
match what you type.  You can then select the Terminal from the results list and
the Terminal will be launched.


### Loading the Terminal upon Start up

One nice feature of the Mac is that we can specify applications to be
automatically launched whenever we initially login to our account.  To access
this using Spotlight, I'll use the Cmd-Space shortcut that we discussed
previously and type "Users & Groups".  Then I can select my login account and
click on the "Login Items" tab.  From there I can click on the + (plus-sign) icon,
which will load up a Finder window, and then I can use the Search field to type
in the word "Terminal" to let the Mac do the work of finding this application
for me.  Once the window is populated with search results, I can select the
Terminal.  Now, whenever my system boots up and I login to my account for the
first time, I should see that the Terminal application is launched automatically
for me.


## Configuring the Terminal

As you can see, the default look of the Terminal is fairly bare bones, so 
before I start using the Terminal on an unfamiliar machine, the first thing that
I like to do is customize it a bit to make the text more readable, as well as
select the colors that suit my preference at the time.  


### Accessing Preferences

So first, let's load up the Terminal Preferences, which can either be accessed
from the Menu bar, or better yet can be accessed using a keyboard shortcut
(command comma):

  Cmd ,

The nice thing about this keyboard shortcut is that it's the same keyboard
shortcut for the Preferences of most Mac applications, not just the Terminal.
So, I would encourage you to memorize this shortcut because you'll find it often
comes in handy.


### Various Terminal Profiles

Once the Terminal Preferences are loaded, be sure to click on the Settings tab.
As you can see, the Mac comes preloaded with a reasonable number of Profile
options to choose from.

If you'd like to see how one looks, just select a profile and open the Terminal.
If you find one you would like to use on a more permanent basis, make it your
default profile by simply clicking the Default option at the bottom.


### Configuring a Terminal profile or theme (optional)

For this tutorial, you're not required to change the profile on your local
system.  It's a completely optional step, so if you're not interested in
learning how to customize the Terminal, please feel free to move on to the next
chapter.

However, I will be making those profile adjustments for this tutorial because
I think it will benefit readability, as well as the fact that I think it's
a useful thing to know how to do.

Because there are so many Terminal profiles that are available for download, I'd
prefer to select one of them instead of using the profiles that come
pre-installed.  Let's go ahead and do that so you can see how to import them
properly.

So, the Terminal profile that I'm using at the moment is the Hemisu theme.  It's
available on Github as a public repository, so let's go download it.


### Importing a profile or theme

The easiest way to handle the importing is by first downloading the zip file
from Github, and then unzipping the archive.  Once that's done, you can just
double-click on any file that ends with a ".terminal" extension.

Another way of importing can be done from the Settings tab of the Terminal
Preferences window.  From there simply clicking the Gear icon will open a window
that allows you to select a Terminal theme.


### Further Terminal settings

Now that the theme is imported, I would like to change the font to Menlo and
then bump it up 24 point type, which I think will be large enough to be easily
readable for the remainder of this tutorial.

Additionally, from the Text tab you also have the ability to customize the
Colors as well as tune how the Cursor looks.

From the Window tab you can customize the Title, such as the Active Process Name
or TTY name, as well as set colors or an image to be used on the Background, and
also the Window size and Scrollback.

From the Shell tab you can set parameters for when the shell starts or exits,
as well as whether or not to prompt when the Terminal closes.

From the Keyboard tab, you can see various key mappings and edit them to suit
your specific needs.

Finally, from the Advanced tab you can set your terminal's emulation mode,
whether or not to use an audible or visible Bell, as well as settings for
character encodings.


## Chapter Summary

So that should cover it for this chapter on "Getting started using the
Terminal".  We've covered how to easily access it, as well as how to perform
basic customization.  We also showed how to import a theme or profile into
Terminal so that you can drastically improve the look-and-feel of the
application to get the most out of it.

That should be enough for you easily follow through this tutorial as we explore
more about the UNIX command line.
