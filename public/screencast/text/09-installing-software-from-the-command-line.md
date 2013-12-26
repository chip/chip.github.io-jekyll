# Installing software on OS X

Even though the Mac comes pre-installed with a lot of great software, it
doesn't have everything you normally need, so it's essential to
understand how to install new software on the system so that you can
customize it accordingly.


## The AppStore

In more recent versions of OS X, you can use the AppStore to install
some applications.  Let's launch it from Spotlight to take a quick look.
As you can see, applications from Apple such as Xcode and iPhoto are
available, as well as software from other vendors, including apps like
Evernote, Pixelmator and many others.

What's nice about the AppStore is that it makes upgrading software very
easy to manage.  Since there are a lot of applications that are not
available through the AppStore, it's important that we look at other
ways to install software on your system.

<!-- Registered trademarks need to be specified somewhere -->


## Downloading an application

There are number of applications that are very easy to install on the
Mac.  All that's necessary is to download them and be sure they are
copied to the `/Applications` directory on your system.  

To show you an example of this approach, I'm going to install one of my
personal favorite applications called **Alfred**, which is a nice
replacement for the Mac's **Spotlight** feature.

Let's follow these steps to install the application:

1.  Point our browser to software vendor site, which in this example is
    http://www.alfredapp.com/.

2.  We'll then download the app, which comes packaged as a zip file.

3.  By clicking on that zip file, the Mac will automatically unzip it for
    us to our `Downloads` directory.

4.  To complete the installation, we'll copy the file to the `/Applications`
    directory.  Of course, we'll need enter our Administrator's credentials to
    finish the file copying process.

Occasionally, you'll come across an application that provides an
installation wizard which will handle the process of copying the files to
their appropriate destinations, so in those cases, manually copying a file
to the `/Applications` directory might night be necessary.


## Installing from a binary

Although many software applications for the Mac are either available via
the AppStore or downloadable from a vendor's website as a zipped
archive, many are pre-packaged as OS X Disk Image files that end in the
file suffix **.dmg**.

Generally, this approach to installing software on the Mac is super
simple, so let's look at an example.

One application that I've come to use more often when I'm working with
relational databases such as [MySQL](http://mysql.com) or
[PostgreSQL](http://www.postgresql.org/) is [Sequel
Pro](http://www.sequelpro.com/download).  Let's visit the Sequel Pro
website to see how this process works for installing a Disk Image file.

First, I'll click "Download Now", and then wait a few seconds for it to
download.  Once the file has finished being transferred to my machine,
I'll click on the Disk Image file to mount it on my file system.  If you
launch the **Finder**, you'll see that it presents the Disk Image as
a device on your system, much like a hard drive.  You can then click on
the mounted Disk Image and drag the Sequel Pro file over to the
Applications folder.  You will probably need to enter your systems
Administrator's credentials to successfully copy the file.

Upon launching the application, you might also see a window pop up
informing you that this is an application downloaded from the Internet,
confirming that you want to open it.  In this case I'm ok with that
change, so I click "Open" to proceed with running the application.  When
you're done, you can unmount the Disk Image by clicking on the eject
icon for the device.

Other applications offer a slightly different route upon launching
a Disk Image file, ranging from a simple double-click of the application
to walking you through an installation wizard where it asks you to
authenticate as your machine's administrator, then by it prompting for
the destination drive of where the application should be installed,
followed by a series of other steps to complete the process.

Since the former of those routes is more and more common, I'll show you
a brief example using the popular [Sublime Text
2](http://www.sublimetext.com/2) editor.  First, we'll visit the site,
click on the download, and when it completes we'll open the Disk Image
file.  From there we can simply double-click on the downloaded file,
except this time we don't have to manually open the **Finder** to
install the app, since the Disk Image provides an **Applications**
folder for us.  To complete the installation, we simply drag the Sublime
Text 2 file over to the **Applications** folder and, if necessary,
authenticate using our Administrator's credentials.


## Using curl

Now that we have the traditional software installation methods out of
the way, it's time to get back to the command line.  Let's follow an
example of installing software from there, except this time we're going
to use a common OS X application called `curl`.  The `curl` utility
essentially allows you to transfer a file from a specified url on the
Internet to your local machine.  
 
What I'd like to do is download a great application from
[37signals](http://37signals.com) called [Pow](http://pow.cx), which is
a Rack-based web server for the Mac.  According to the installation
instructions, we'll use `curl` to fetch a file and then pipe the
downloaded data to the shell, which will then run the file as if it were
a shell script already on our file system.

Before blindly doing something like that, I would advise you to visit
the site first with your web browser or simply download the file with
`curl` to inspect its contents.

Let's use our browser to do that, and I'll go to the site to inspect the
shell script's source code (http://get.pow.cx).  Since I've run this
script many times and know it comes from a trusted source, I'm
comfortable with running the installation command provided on the site:

    iMac:~ screencast$ curl get.pow.cx | sh

It should provide feedback as it continues through the installation
process:

      % Total    % Received % Xferd  Average Speed   Time    Time     Time
    Current
                                     Dload  Upload   Total   Spent    Left
    Speed
    100  6887  100  6887    0     0  12930      0 --:--:-- --:--:-- --:--:--
    59886
    *** Installing Pow 0.4.1...
    *** Installing local configuration files...
    /Users/screencast/Library/LaunchAgents/cx.pow.powd.plist
    *** Installing system configuration files as root...
    /Library/LaunchDaemons/cx.pow.firewall.plist
    *** Starting the Pow server...
    *** Performing self-test...
    *** Installed

    For troubleshooting instructions, please see the Pow wiki:
    https://github.com/37signals/pow/wiki/Troubleshooting

    To uninstall Pow, `curl get.pow.cx/uninstall.sh | sh`

What's great about this script's installation is that it provides a link
to the documentation, as well as instructions for un-installing the
application.

As you can see, installing software using `curl` is usually
straightforward, but it's not the most common way. We'll need to look at
other ways to manage software on our system if we're going to continue
customizing it.


## Package management

On most UNIX systems, there is usually a package management system
available to make it easier to find software, install, upgrade and
remove it.  Fortunately, the Mac is no different and provides more than
one way to handle this.

Here are a few of the popular package management systems that I've used
over the years:

    1. Fink - http://www.finkproject.org/

    2. MacPorts - http://www.macports.org/

    3. Homebrew - http://brew.sh/

When I bought my first Mac, I downloaded
[fink](http://www.finkproject.org) because it was fairly common at the
time.  For my own needs, it worked really well.  However, over time
I noticed more developers referring to other package management systems
such as [MacPorts](http://www.macports.org/).  Since it appeared to have
a repository full of the software packages I needed, I felt it was
a great option.  Over time, I felt that I ran into issues from time to
time with the software.  Since I was working a lot with
[Ruby](http://www.ruby-lang.org) at the time, and it appeared that many
other developers ran into the same issues that I did, I always felt
there was room for a better system. 

To be fair, I haven't used those systems in several years.  I wouldn't
be surprised to hear that those projects have greatly improved, but
since I have since moved on to [Homebrew](http://brew.sh/), or just
**Brew**, I find that I don't have a need to try any other package
management system.

In my opinion, **Brew** is the best package management system available
for OS X, so I encourage you to try it.  However, if for some reason it
doesn't work well for your needs, it's good to know that you have at
least a couple more options available to try.

  Site note: Remove existing package management systems before
             installing new ones.

One caveat to keep in mind is anytime you select a package management
system, be sure that you fully remove any other package management
systems before proceeding with installing a new one.  In my experience,
attempting to commingle various package management systems together can
have adverse effects on your system.

For that very reason, I won't be installing **Fink** or **MacPorts**
here in this tutorial, but instead I opt to cover some of the basics of
**Brew**.


## Homebrew basics

Since I've started using **Brew** I've run into fewer problems with
software package management on my Mac.  For most packages, I've found
that it "just works", so I've been quite happy with it.

Before I cover the basics of using **Brew**, I would like to encourage
anyone following this tutorial to refer to the website for the most up
to date documentation on how to use it effectively, as the commands that
follow could potentially change in the future.  Alternatively, you can
use the help provided by **Brew** or refer to it's man page:

    brew help

    man brew

At the time of this writing, the installation of **Brew** requires the
following command:

    ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"

Since I already have it installed on my machine, I'll be skipping this
step, but please feel free to pause for a moment and install it on your
local machine if you want to follow along.


### brew update

For my system, I'd like to be sure that I've got the latest version of
the system, so I'll perform an update of it:

    brew update

And we can see that it proceeds with updating the utility:

    Updated Homebrew from 19a59a9f to a8cdb00e.
    ==> New Formulae
    homebrew/versions/llvm34     polygen        stlviewer
    ==> Updated Formulae
    adobe-air-sdk        drush        gobject-introspection
    homebrew/dupes/gdb      kytea      netpbm           pyqt
    terminal-notifier
    bash           elixir       gpg-agent          homebrew/versions/gcc49
    lftp       nexus            python         tiger-vnc
    boost          eventlog       gst-libav
    homebrew/versions/llvm31      libcouchbase     node           python3
    uwsgi
    calc           fish       gst-plugins-bad
    homebrew/versions/llvm32      libdlna      openssl          qemu
    xmlto
    chruby           freeglut       gst-plugins-base
    homebrew/versions/llvm33      llvm       osm2pgsql          redis
    yaz
    denominator        gettext        gst-plugins-good         icu4c
    mackup       passenger          rethinkdb        youtube-dl
    djvulibre        ghc        gst-plugins-ugly         isl          macvim
    pngquant          rtmpdump         zeromq
    dnscrypt-proxy         git        gstreamer          ispell
    metaproxy      poppler          sip
    doxygen          gnupg2       hevea            jenkins          mongodb
    pypy            storm


### brew outdated

Now that I've upgraded to the latest version, I'd like to see what
packages on my system have newer versions available for upgrade, so I'll
do a check using the outdated argument:

    brew outdated

Your system will certainly vary, but mine responded with quite a number
of packages that need upgrading:

    ack (1.96 < 2.04)
    automake (1.13.1, 1.13.4 < 1.14)
    bazaar (2.5.1 < 2.6.0)
    cmake (2.8.7, 2.8.8, 2.8.10.1, 2.8.11.1 < 2.8.11.2)
    fontconfig (2.10.1 < 2.10.93)
    freetype (2.4.10 < 2.4.11)
    gettext (0.18.1.1 < 0.18.3.1)
    ghostscript (9.05, 9.06 < 9.07)
    gist (3.1.0 < 4.0.3)
    git (1.7.9.5, 1.8.1 < 1.8.4)
    graphviz (2.28.0 < 2.32.0)
    imagemagick (6.7.7-6 < 6.8.6-3)
    libgpg-error (1.10 < 1.12)
    libksba (1.2.0 < 1.3.0)
    libpng (1.5.13 < 1.5.14)
    little-cms2 (2.3, 2.4 < 2.5)
    mercurial (2.2.1, 2.4.1 < 2.7)
    mysql (5.5.20, 5.5.29 < 5.6.13)
    pcre (8.30, 8.31, 8.32 < 8.33)
    phantomjs (1.5.0, 1.7.0, 1.8.1 < 1.9.1)
    postgresql (9.1.3, 9.2.2 < 9.2.4)
    pv (1.3.9 < 1.4.6)
    qt (4.8.0, 4.8.4 < 4.8.5)
    reattach-to-user-namespace (2.1 < 2.2)
    redis (2.4.13, 2.6.7 < 2.6.15)
    ruby-build (20130104 < 20130806)
    ssh-copy-id (5.9p1, 6.0p1 < 6.2p2)
    swig (2.0.6, 2.0.9 < 2.0.10)
    the_silver_searcher (0.13.1 < 0.15)
    tmate (1.8.5 < 1.8.8)
    tmux (1.6, 1.7 < 1.8)
    vim (7.3.666, 7.3.762 < 7.4)


### brew upgrade

Let's say in this case I wanted to upgrade `ack`, which is a really
great alternative to `grep` for quickly searching through files, I would
upgrade it like this:

    brew upgrade ack

And it shows you a nice thorough output of the results:

    ==> Upgrading 1 outdated package, with result:
    ack 2.04
    ==> Upgrading ack
    dyld: DYLD_ environment variables being ignored because main executable
    (/usr/bin/sudo) is setuid or setgid
    ==> Downloading https://github.com/petdance/ack2/archive/2.04.tar.gz
    ########################################################################
    100.0%
    ==> pod2man /usr/local/Cellar/ack/2.04/bin/ack ack.1
    🍺  /usr/local/Cellar/ack/2.04: 6 files, 196K, built in 2 seconds

As you can see from the output, **Brew** installs all software under the
`/usr/local/Cellar` directory by default.  That location can be changed,
but according the FAQ on the site, I would recommend sticking with the
defaults.  In my own experience, the default setting has been sufficient
for my needs, so I don't anticipate that presenting any problems for
you.

If I would rather upgrade every software package that **Brew** manages
instead of specifying each one individually, I can just use the
`upgrade` argument by itself:

    brew upgrade

Be advised that this process might take a while to complete.

Once that's done, you can always get a full list of the software
packages installed by **Brew** using the `list` argument:


### brew list

    brew list

And here's it's output from my system:

    ack           libxslt
    apple-gcc42   little-cms
    autoconf      little-cms2
    automake      mercurial
    bazaar        mysql
    cmake         openssl
    ctags         optipng
    dmenu         ossp-uuid
    fontconfig    pcre
    fortune       phantomjs
    freetype      pidof
    gdbm          pkg-config
    gettext       postgresql
    ghostscript   psgrep
    gifsicle      pv
    gist          qt
    git           env
    graphviz      readline
    html2text     reattach-to-user-namespace
    htop-osx      redis
    imagemagick   ruby-build
    ispell        ssh-copy-id
    jasper        swig
    jbig2dec      the_silver_searcher
    jpeg          tmate
    libevent      tmux
    libgpg-error  tree
    libksba       vim
    libpng        watch
    libtiff       wemux
    libtool       wget
    libxml2       zsh


### brew doctor

If you ever suspect that something is out of whack with **Brew**, or
you're having trouble installing a package, it's always nice to call the
doctor:

    brew doctor

It's a good thing that I just ran that command, because it informed me
of several problems with my system and was nice enough to tell me the
commands necessary to fix those issues:

    Warning: Your XQuartz (2.7.3) is outdated
    Please install XQuartz 2.7.4.

    Warning: Some installed formula are missing dependencies.
    You should `brew install` the missing dependencies:

        brew install xz

    Run `brew missing` for more details.

    Warning: Your compilers are different from the standard versions for
    your Xcode.
    If you have Xcode 4.3 or newer, you should install the Command Line
    Tools for
    Xcode from within Xcode's Download preferences.
    Otherwise, you should reinstall Xcode.
    Error: Failed to import: perl
    No available formula for perl 


### brew missing

As the output indicated, I can use the following to determine any
missing dependencies:

    brew missing

After running that command, it shows exactly why I need to install the
`xz` package - because `the_silver_searcher` package needs it:

    the_silver_searcher: xz


### brew search

All of the `brew` commands covered so far are great, but my general work
flow is far simpler.  I usually just want to find the software I need,
install it to try it out, and then remove it if I feel it isn't helpful
or necessary.

So let's search for a package.  In this example, I'm going to search for
`sql`, in an effort to find a relational database package:

    brew search sql

It returns a wide array of database packages for me to choose from:

    automysqlbackup                       mysql-connector-c++    postgresql
    google-sql-tool                       mysql-connector-odbc   postgresql8
    groonga-normalizer-mysql              mysql-proxy            postgresql9
    mysql                                 mysql51                postgresql91
    mysql++                               mysql55                sqlcipher
    mysql-cluster                         mysqlreport            sqlite
    mysql-connector-c                     osm2pgsql              sqliteman
    josegonzalez/php/php53-mysqlnd_ms     josegonzalez/php/php55-mysqlnd_ms
    josegonzalez/php/php54-mysqlnd_ms


### brew install

In this example, I've decided to install`sqlite`, which I find quite
useful in smaller [Rails](http://rubyonrails.org) or
[Sinatra](http://sinatrarb.com) projects, so I'm going to move forward
with the installation:

    brew install sqlite

As usual, the output shows the progress:

    ==> Downloading http://sqlite.org/2013/sqlite-autoconf-3071700.tar.gz
    ########################################################################
    100.0%
    ==> ./configure --prefix=/usr/local/Cellar/sqlite/3.7.17
    --enable-dynamic-extensions
    ==> make install
    ==> Caveats
    This formula is keg-only: so it was not symlinked into /usr/local.

    Mac OS X already provides this software and installing another version
    in parallel can cause all kinds of trouble.

    OS X provides an older sqlite3.

    Generally there are no consequences of this for you. If you build your
    own software and it requires this formula, you'll need to add to your
    build variables:

        LDFLAGS:  -L/usr/local/opt/sqlite/lib
        CPPFLAGS: -I/usr/local/opt/sqlite/include

    ==> Summary
    🍺  /usr/local/Cellar/sqlite/3.7.17: 9 files, 2.0M, built in 21 seconds


### brew uninstall

If I ever want to remove this package, I would simply run the
following command:

    brew uninstall sqlite

So that's the basic overview of **Brew**.  I've found it to be
a terrific tool for package management on OS X, so hopefully you'll give
it a try and have similar success.


## Working with compressed archive files

Before we install any software from source, we'll need to have a basic
understanding of how to work with compressed or archived files.  Here
are some of the more common compressed file format extensions that we'll
cover:

    .zip

    .gz

    .tar

    .Z


#### zip

To create files with a `.zip` extension, we need to use the `zip`
utility, which is capable of packaging and compressing files.

Viewing the contents of our **Downloads** directory, we can see that we
have a couple of files to work with:

    $ ls -l ~/Downloads
    total 8312
    -rw-r--r--@ 1 screencast  staff     9642 Jul 30 15:32 Hemisu-Dark.terminal
    -rw-r--r--@ 1 screencast  staff  4239836 Aug 26 21:46 tk8.6.0-src.tar.gz

Let's compress them using `zip` to see it in action, but let's specify
`laptop` as the name of the archive:

    $ zip laptop ~/Downloads/\*
      adding: Users/screencast/Downloads/Hemisu-Dark.terminal (deflated 82%)
      adding: Users/screencast/Downloads/tk8.6.0-src.tar.gz (deflated 0%)

As you can see the files were compress using the archive name we
specified, but `.zip` was added as a file suffix:

    $ ls -l laptop.zip 
    -rw-r--r--  1 screencast  staff  4238869 Aug 26 21:55 laptop.zip


### unzip

In contrast to `zip`, we have the `unzip` utility, which gives us the
ability to list, test and extract compressed files in a ZIP archive
format. By specifying the `-l` option, we can simply list the contents
of the archive created previously:

    $ unzip -l laptop.zip 
    Archive:  laptop.zip
      Length     Date   Time    Name
     --------    ----   ----    ----
         9642  07-30-13 15:32
    Users/screencast/Downloads/Hemisu-Dark.terminal 4239836  08-26-13 21:46
    Users/screencast/Downloads/tk8.6.0-src.tar.gz
     --------                   -------
      4249478                   2 files

Instead of simply listing the contents of the `zip` archive, we can
extract the files:

    $ unzip laptop.zip

We can also explicitly specify the target directory to which the zipped
files should be extracted using the `-d` option:

    $ unzip laptop.zip -d /tmp
    Archive:  laptop.zip
      inflating: /tmp/Users/screencast/Downloads/Hemisu-Dark.terminal  
      inflating: /tmp/Users/screencast/Downloads/tk8.6.0-src.tar.gz 

Here we've told the `unzip` utility to extract, or "inflate", the files
to the `/tmp` directory.


### gunzip

An alternative to using `unzip` is another archive extraction utility
called `gunzip`, which expands files that were encoded using the
Lempel-Ziv algorithm.  Let's take a look at an example by revisiting the
file listing for the **Downloads** directory:

<!-- The prompt needs to be fixed -->

    $ ls -l ~/Downloads
    total 8312
    -rw-r--r--@ 1 screencast  staff     9642 Jul 30 15:32 Hemisu-Dark.terminal
    -rw-r--r--@ 1 screencast  staff  4239836 Aug 26 22:04 tk8.6.0-src.tar.gz

As you can see, we already have a file with the `.gz` extension,
indicating that it might be an archive created in gzipped format, but
let's verify that to be sure using the `file` utility:

    $ file ~/Downloads/tk8.6.0-src.tar.gz 
    /Users/screencast/Downloads/tk8.6.0-src.tar.gz: gzip compressed data,
    was "tk8.6.0rc5-src.tar", from Unix, last modified: Wed Dec 19 09:39:30
    2012, max compression

Since it is indeed a gzipped file, let's extract the contents using
`gunzip`:

    $ gunzip ~/Downloads/tk8.6.0-src.tar.gz

The end result is a much larger file, but one that is still compressed
using an altogether different format called `tar`:

    $ ls -l ~/Downloads/
    total 36904
    -rw-r--r--@ 1 screencast  staff      9642 Jul 30 15:32 Hemisu-Dark.terminal
    -rw-r--r--@ 1 screencast  staff  18882560 Aug 26 22:04 tk8.6.0-src.tar


### tar

The `tar` utility has been around for several decades and was originally
used to write data to devices for tape backup purposes, but today it is
still used for packaging files into one larger file for distribution or
archiving.

First, let's simply list the contents of the `tar` archive, without
extracting the files:

    $ tar tf ~/Downloads/tcl8.6.0-src.tar
    tcl8.6.0/
    tcl8.6.0/ChangeLog.2008
    tcl8.6.0/ChangeLog.2003
    tcl8.6.0/tools/
    tcl8.6.0/tools/man2html.tcl
    tcl8.6.0/tools/tcl.hpj.in
    tcl8.6.0/tools/man2html1.tcl
    tcl8.6.0/tools/genStubs.tcl
    tcl8.6.0/tools/tcltk-man2html-utils.tcl
    tcl8.6.0/tools/makeTestCases.tcl
    .
    .
    .

Looking at the man pages we can see that the `t` option is used to to
list archive contents to stdout, while the `f` option is used to specify
the filename of the archive to be read.

To the astute observer, you might have noticed that flags normally
require a preceding `-` character represented by a hyphen.  However, for
compatibility reasons, the tar command supports what's known as the
bundled-arguments format.

In other words, all of the following are equivalent commands:

    $ tar tf ~/Downloads/tcl8.6.0-src.tar

    $ tar -tf ~/Downloads/tcl8.6.0-src.tar

    $ tar -t -f ~/Downloads/tcl8.6.0-src.tar

Now that I know the contents of the tar archive (or tar file), I've
decided to extract the contents of it:

    $ tar xvf ~/Downloads/tcl8.6.0-src.tar 
    x tcl8.6.0/
    x tcl8.6.0/ChangeLog.2008
    x tcl8.6.0/ChangeLog.2003
    x tcl8.6.0/tools/
    x tcl8.6.0/tools/man2html.tcl
    x tcl8.6.0/tools/tcl.hpj.in
    x tcl8.6.0/tools/man2html1.tcl
    x tcl8.6.0/tools/genStubs.tcl
    x tcl8.6.0/tools/tcltk-man2html-utils.tcl
    x tcl8.6.0/tools/makeTestCases.tcl

The `x` flag is used to extract the archive file to disk, while the `v`
flag is used to display output in a verbose format, which basically
lists the files as they're being extracted.  As before in the previous
command, the `f` option is used to specify the archive file name to be
used during the extraction.

As you can see, the files were indeed extracted to the directory:

    $ ls -l ~/Downloads/tcl8.6.0
    total 3952
    -rw-r--r--@   1 screencast  staff  313082 Dec 17  2012 ChangeLog
    -rw-r--r--@   1 screencast  staff   91145 Apr 26  2011 ChangeLog.1999
    -rw-r--r--@   1 screencast  staff   93933 Nov  7  2012 ChangeLog.2000
    -rw-r--r--@   1 screencast  staff  138130 Nov  7  2012 ChangeLog.2001
    -rw-r--r--@   1 screencast  staff  181491 Apr 26  2011 ChangeLog.2002
    -rw-r--r--@   1 screencast  staff  130445 Nov  7  2012 ChangeLog.2003
    -rw-r--r--@   1 screencast  staff  181262 Apr 26  2011 ChangeLog.2004
    -rw-r--r--@   1 screencast  staff  145731 Apr 26  2011 ChangeLog.2005
    -rw-r--r--@   1 screencast  staff  226505 Apr 26  2011 ChangeLog.2007
    .
    .
    .


### Creating tar archives

So how are `tar` archives created?  Let's find out.  First, we'll create
a couple of small files just to show a quick example:

    $ echo one > one
    $ echo two > two

And we'll take a look at those files just to verify the contents:

    $ cat one two
    one
    two

And then we'll use `tar` to create the archive:

    $ tar c one two > numbers

First, you'll notice that the `c` flag is passed, which is used to
create a new archive containing the specified items, and then we specify
the names of the files or directories that we want included into the
archive.  Finally, we redirect the output to a file called **numbers**.


### The `file` command

Let's verify the files' type using the `file` command:

    $ file numbers 
    numbers: POSIX tar archive

And we can see that it is indeed a tar archive file.

However, we didn't follow the convention of specifying the `.tar` file
suffix when we created the archive, so it wasn't easy for us to tell
what type of file it was without using the `file` command.  Instead, I'd
rather follow the convention to make it easier on myself and others who
view this file later on, so let's first clean up our mess by removing
the file and then recreate it using the convention:

    $ rm numbers 
    $ tar c one two > numbers.tar

Again, let's use the file command to verify the file type:

    $ file numbers.tar 
    numbers.tar: POSIX tar archive

We're looking good there, so let's clean up again:

    $ rm numbers.tar 


### Creating a gzipped tar archive

If I wanted to `gzip` the contents of a tar file, I could easily do that
after creating the archive, but sometimes it would be nice to create
that format in one shot.  Let's do that, but this time we're going to
create the archive using the `z` flag, which tells tar to compress the
file in `gzip` format after creating the `tar` archive:

    $ tar cz one two > numbers.tar.gz

Verifying the contents of the file, we can see that it is compressed
using `gzip`, but there's no evidence of this file being a `tar`
archive:

    $ file numbers.tar.gz 
    numbers.tar.gz: gzip compressed data, from Unix, last modified: Wed
    Sep 11 15:48:58 2013

To do that, we'd need to uncompress it first using `gunzip`:

    $ gunzip numbers.tar.gz 

And as you can see, the resulting file contains only a `.tar` file
suffix and is in the expected format:

    $ file numbers.tar 
    numbers.tar: POSIX tar archive

    $ rm numbers.tar 


### Alternative file suffix: .tgz

As a side note, occasionally you'll come across files that have a `.tgz`
suffix, which is essentially a shorthand format of specifying `.tar.gz`.
Just remember, compressed archive files can be created just the same as
before using this alternative:

    $ tar cz one two > numbers.tgz
    $ file numbers.tgz 
    numbers.tgz: gzip compressed data, from Unix, last modified: Wed Sep
    11 15:49:36 2013

As before, it's a `gzip` compressed file.  We can uncompress it, just as
before and verify it's contents:

    $ gunzip numbers.tgz 
    $ file numbers.tar 
    numbers.tar: POSIX tar archive

    $ rm numbers.tar 


### Extracting a gzipped tar archive

Just as you can create gzipped tar archive files using one command, you
can also extract them in one shot.  Let's recreate that archive:

    $ tar cz one two > numbers.tar.gz

This time, let's extract it using the same flags as before, except this
time we'll specify the `z` flag so that we can skip the separate
step using `gunzip`:

    $ tar xvfz numbers.tar.gz 
    x one
    x two

As before, keep in mind that the alternative `.tgz` file format works
exactly the same way:

    $ tar cz one two > numbers.tgz
    $ tar xvfz numbers.tgz 
    x one
    x two


### Using compress and uncompress

Moving on to our last compressed file format, you'll occasionally see
files with a `.Z` suffix.  This type of file is created using adaptive
Lempel-Ziv coding.  This file format isn't as common as the others
previously shown, but I'd like to show you the basics to help you get
started.  

Taking a look at a sample file, created in Markdown format, we can see
that the file's size is 5,682 bytes:

    $ ls -l readme.md 
    -rw-r--r--  1 screencast  staff  5682 Jul 22 15:23 readme.md

To compress the file, simply use the namesake command, `compress`, and
then specify the filename(s) you want compressed:

    $ compress readme.md 

This creates a file of the same name, but with the `.Z` suffix:

    $ ls -l readme.md.Z 
    -rw-r--r--  1 screencast  staff  3319 Jul 22 15:23 readme.md.Z

Also notice that the file size has shrunk down to 3,319 bytes due to the
compression used.

Alternatively, we can use the `uncompress` command to reverse the
process and leave us with the original file:

    $ uncompress readme.md.Z 
    $ ls -l readme.md 
    -rw-r--r--  1 screencast  staff  5682 Jul 22 15:23 readme.md

So that covers some of the most common file formats in UNIX which use
compression.  For each command covered, please dig into the man pages
for more details.


## Installing from source

To wrap up this chapter, we're going to briefly cover installing
software from source code.

Using the download from earlier for the **Tcl** language, I'm going to
change into that directory so we can perform the installation:

    $ cd ~/Downloads/tcl8.6.0

Usually you'll find a **README** or **INSTALL** file in most common UNIX
packages, so I encourage you to always read those files first, as they have a lot
of details that might be important depending on the type or version of
system you have installed.

For this package, it refers me to the online installation instructions,
so let's go there and check them out.  From there it tells me to change
into the **unix** directory before proceeding with the installation, as
there are a number of target platforms supported for the **Tcl**
language.

The basic approach for installation **Tcl** from source is the following:

    $ ./configure

    $ make

    $ make test

    $ make install

This process is not unique to the **Tcl** language.  In fact, it's
generally the same steps for any UNIX package which you'll install from
source.

So, let's break down each step.

    $ ./configure

Running the `configure` script is usually done to analyze your system
and prepare it for selecting various packages to include in the build,
as well as other details such as specifying another target installation
directory other than the default.

    $ make

Running the `make` command is very common in UNIX and works by executing
commands based on the definitions specified in a file called `Makefile`.

    $ cat Makefile

Those commands essentially build the software in the local directory
tree.  

Once the `make` command has completed, you essentially have the software
built on your system, but it is not installed in the proper location.

Before doing that, you'll sometimes see a step called `make test`, which
essentially runs the `Makefile`, but this time runs a series of tests to
ensure that the software that you just built is compatible with your
system.  Some software packages don't offer this step for running `make
test`, so don't be surprised if it's not available.

    $ make test

If the tests all passed, or if you're comfortable with installing the
software anyway, you can proceed with the last step, which is `make
install`:

    $ make install
    Installing libtcl8.6.dylib to /usr/local/lib/
    cp: /usr/local/lib/_inst.31022_: Permission denied
    make: *** [install-binaries] Error 1

However, there's a problem.  You'll need to have the proper privileges
to install it on your system, so you'll usually need to be logged in as
the `root` user, also known as the superuser or administrator of the
system.  Alternatively you'll need to prefix the `make install` command
using the `sudo` command:

    $ sudo make install
    Installing libtcl8.6.dylib to /usr/local/lib/
    Installing tclsh as /usr/local/bin/tclsh8.6
    Installing tclConfig.sh to /usr/local/lib/
    Installing tclooConfig.sh to /usr/local/lib/
    Installing libtclstub8.6.a to /usr/local/lib/
    Installing pkg-config file to /usr/local/lib/pkgconfig/
    Making directory /usr/local/lib/tcl8.6
    Making directory /usr/local/lib/tcl8.6/opt0.4
    Making directory /usr/local/lib/tcl8.6/http1.0
    Making directory /usr/local/lib/tcl8.6/encoding
    .
    .
    .

Of course, you'll either need to know the administrator's password or upgrade
your standard user account to an administrator's level for this to work.  

If all went well, you should have the software installed on your system.
Let's try that out by launching the `tclsh`:

    $ tclsh
    % 

<!-- Add in output from the tclsh help command -->

As you can see, it appears that the installation for Tcl was successful.

So that wraps up this chapter on installing software on OS X.  Hopefully
you now have the basics under your belt to be able to install software
using a variety of approaches.  Remember to always consult the
software's documentation for installation specifics, as well the man
pages for the commands you use so you can resolve errors if you run
into them.
