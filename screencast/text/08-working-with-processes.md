# UNIX processes

To start of this section, let's first think about the definition of a process.

    A process is an instance of a computer program that is being executed

Examples of this include any time you run a shell command such as `ls`
, `cd`, or many other built-in commands, or when you launch an
application from the OS X Finder.


## Viewing running processes

To see evidence of this, we have a couple of commands that are
especially helpful in viewing running processes.

### ps - process status

The first command is `ps`, which shows a process status.  

    iMac:~ screencast$ ps
      PID TTY           TIME CMD
    71647 ttys000    0:00.10 -bash
    15707 ttys006    0:00.03 -bash
    81370 ttys006    0:03.84 vi 08_working_with_processes.md
    17427 ttys007   12:08.23 node /usr/local/bin/grunt serve
    71133 ttys007    0:00.01 -bash

By default, this command shows one process per line, with the format of
each line being represented by the process id (PID), followed by the
controlling terminal of the process (TTY), the CPU time (TIME), and the
command that the process represents (CMD). 
 
A popular option for the `ps` command is to use the `-f` flag, which
shows a slightly different output:

    iMac:~ screencast$ ps -f
      UID   PID  PPID   C STIME   TTY           TIME CMD
      503 71647 71646   0 Sat05PM ttys000    0:00.10 -bash
      503 15707 15705   0 Mon06PM ttys006    0:00.03 -bash
      503 81370 15707   0  1:00PM ttys006    0:03.84 vi 08_working_with_processes.md
      503 17427 71133   0 Thu10AM ttys007   12:08.24 node /usr/local/bin/grunt serve
      503 71133 71132   0 Sat04PM ttys007    0:00.01 -bash

It's important to note that this format shows the user id (UID) that
owns the process, as well as the parent process id (PPID) and the
starting time for the process (STIME).

But let's say that neither the default output of the `ps` command, nor
using the `-f` flag is suitable for your needs.  In addition to checking
the man pages for a wide array of other flags to use, you can also setup
a customized output using the `-o` flag.

In the following example, I'd like the `ps` command to simply give me
the user that owns the process, followed by the process id, also known
as a "pid", the status or state of the process, and finally the command
that the process represents:

    iMac:~ screencast$ ps -o user,pid,state,command
    USER         PID STAT COMMAND
    screencast 71647 S    -bash
    screencast 15707 S    -bash
    screencast 81370 S+   vi 08_working_with_processes.md
    screencast 17427 S+   node /usr/local/bin/grunt serve
    screencast 71133 S    -bash

What's interesting to note here is that the status column can tell us
a number of things.  First, I have several processes running the bash
shell that have an `S` status (STAT), which means that these processes
are sleeping.  The other processes are also sleeping, but the plus sign
`+` indicates that these processes are in the foreground of their
controlling terminal.

So far we've only looked at a list of processes that are owned by the
user we're logged in as, which is the screencast user.

If we needed to look at processes of other users on the system, we would
need to pass another flag to the `ps` command.  Using the `-a` flag will
do just that: 

    iMac:~ screencast$ ps -a
      PID TTY           TIME CMD
    71646 ttys000    0:00.02 login -pfl screencast /bin/bash -c exec -la bash /bin/bash
    71647 ttys000    0:00.11 -bash
    83479 ttys000    0:00.00 ps -a
    54558 ttys001    0:00.25 -zsh
     4122 ttys002    0:09.10 /Users/chip/.rbenv/versions/1.9.3-p194/bin/ruby script/rails c
    55366 ttys002    0:02.13 -zsh
     3419 ttys003    0:00.74 -zsh
    79831 ttys004    0:00.22 -zsh
    27149 ttys005    0:00.32 login -pf chip
    27151 ttys005    0:00.18 -zsh
    27346 ttys005    0:00.01 tmux attach -t wobo
    15705 ttys006    0:00.21 login -pfl screencast /bin/bash -c exec -la bash /bin/bash
    15707 ttys006    0:00.03 -bash
    81370 ttys006    0:03.84 vi 08_working_with_processes.md
    17427 ttys007   12:08.31 node /usr/local/bin/grunt serve
    71132 ttys007    0:00.02 login -pfl screencast /bin/bash -c exec -la bash /bin/bash
    71133 ttys007    0:00.01 -bash
    18567 ttys008    0:21.94 ruby /Users/chip/.rbenv/versions/1.9.3-p194/bin/rake konacha:serve
    69424 ttys008    0:00.17 -zsh
    71268 ttys009    1:17.39 /Users/chip/.rbenv/versions/1.9.3-p194/bin/ruby script/rails s
    96278 ttys009    0:00.09 -zsh

As you can see there are a number of other processes that I'm running on
another account such as `zsh` for the z shell, `vi` for the vim text
editor, and a couple of `ruby` processes for running Coffeescript specs
and a web server for a Rails application that I'm developing.

Using the `ps` command is a useful tool for getting a list of processes
on your system and has a variety of flags that can be combined to slice
and dice that information is displayed.

However, `ps` can only provide information about processes at that very
moment in which the command is executed.  In other words, it is merely
a snapshot of the state of the processes at a given time.  To have more
up-to-date data, we'll need to use another command that provides process
information in real-time.


### top - display and update sorted information about processes

The `top` program periodically displays a sorted list of system processes.

Let's take a look at top running on our system,  *with the output being reformatted to fit the page*:

    Processes: 249 total, 4 running, 245 sleeping, 1226 threads
    Load Avg: 1.29, 1.07, 0.96  CPU usage: 0.97% user, 2.60% sys, 96.41% idle 
    SharedLibs: 13M resident, 9504K data, 0B linkedit. MemRegions: 96571 total, 5398M resident, 174M private, 938M shared.  
    PhysMem: 1719M wired, 7736M active, 1808M inactive, 11G used, 5113M free.
    VM: 575G vsize, 1054M framework vsize, 35424641(0) pageins, 495(0) pageouts.
    Networks: packets: 37877273/33G in, 25689996/8010M out.  
    Disks: 2748035/66G read, 10767201/273G written.

    PID    COMMAND      %CPU TIME     #TH   #WQ  #PORTS #MREGS RPRVT  RSHRD  RSIZE  VPRVT  VSIZE  PGRP  PPID  STATE    UID
    96278  zsh          0.0  00:00.09 1     0    22     63     3552K  1216K  4740K  21M    2380M  96278 54557 sleeping 501
    91609- Spotify      2.1  02:00:27 27/1  1    422-   737-   119M-  93M    196M-  264M-  1120M- 91609 131   running  501
    88063  coresymbolic 0.0  00:00.50 2     2    28     75     18M    212K   64M    143M   2509M  88063 1     sleeping 0  
    88013- SpotifyWebHe 0.0  00:23.56 3     1    45     87     1336K  14M    3484K  91M    680M   88013 478   sleeping 503
    87731  pomodoro     0.0  04:11.42 5     2    227    218    16M    33M    27M    93M    2538M  87731 478   sleeping 503
    87580  com.apple.Im 0.0  00:02.16 2     2    55     262    101M   7464K  105M   619M   19G    87580 1     sleeping 503
    83910  top          13.1 00:02.79 1/1   0    24     31     2676K  216K   3424K  18M    2377M  83910 71647 running  0  
    83813  ocspd        0.0  00:00.01 1     0    22     27     472K   272K   1924K  17M    2376M  83813 1     sleeping 0  
    82034- Google Chrom 0.0  00:00.51 4     1    91     135    4076K+ 80M    14M+   82M    773M   4453  4453  sleeping 502
    82031- Google Chrom 0.0  00:00.39 11    1    109    244    15M    89M    39M    135M   844M   4453  4453  sleeping 502
    81587- Google Chrom 0.1  00:03.58 10    1    127    354    51M+   98M    84M+   167M   917M   71045 71045 sleeping 503
    81418- Google Chrom 0.0  00:02.60 10    1    127    358    47M    94M    76M    163M   907M   71045 71045 sleeping 503
    81370  vim          0.0  00:08.74 1     0    21     61     3832K  216K   8780K  22M    2385M  81370 15707 sleeping 503
    81032- Google Chrom 0.0  00:04.04 11    2    129    352    49M    94M    81M    150M   919M   71045 71045 sleeping 503
    80942  HPScanner    0.0  00:00.19 4     2    97     115    2696K  16M    8900K  92M    2501M  80942 478   sleeping 503
    80934  ARDAgent     0.0  00:00.03 4     2    56     87     1304K  7444K  4016K  76M    2445M  80934 131   sleeping 501
    80892  mdworker     0.0  00:00.85 4     1    57     95     11M    9500K  18M    92M    2463M  80892 478   sleeping 503

The first few lines of output from the `top` command show a summary of
statistics for your machine, including how many total processes on the
system, how many that are running, the system load average, CPU usage,
memory consumption and other valuable info.

Further down we see that each line of output displays a processes id
(PID), the command associated with that process id, and a number of
other memory and disk-related details for the process, as well as the
parent process id (PPID), process state (STATE) and user id (UID) as
we've seen before when using the `ps` command.

The `top` command can be an important tool for finding which processes
on a system are creating a resource bottleneck, but doing so can be
difficult at times when inspecting the default output.  To more
effectively use `top`, it's helpful to sort the output by a different
column, such as CPU or memory usage.  Let's find out how to do that by
using the Help screen.


### Help screen for top

To access the Help screen when using `top`, just type a question mark
`?`.  Taking a look at the help menu, you'll see that your can sort
processes based on a specific column of output, as well as send signals
to a process, including terminating a process and many others that we'll
cover in a moment.  Anytime you want to exit the Help screen, just type
the letter `q` or press the Enter key and you'll see that you return to
the process list.


### Sorting top output

Let's say my server is sluggish, and I want to see if there is a specific
process that's either maxing out the CPU cycles or simply being a memory
pig.  I can review that by sorting the output, so I'll type the question
mark `?` to load the help screen.  Once I'm there I can see that to sort
by CPU I'll need to type `o` followed by the keyname I'd like to sort
by.  So first I'll press the Enter key to return to the process list,
followed by `o`, and then a prompt is displayed which shows me that
currently the processes are being sorted by `pid`.  I'll type `cpu` and
now I can see the output is sorted by that keyname.


### Update interval for top

Currently `top` is updating it's output in near real-time - basically
1-second intervals.  If I wanted to change the updates to be less
frequent, I can easily do that by typing the `s` key, which will display
a prompt asking me to enter a number.  I'll type 5 and press Enter.  Now
we can see the output is being updated every 5 seconds.


### Sending signals using top

We can also send signals to processes listed from top, such as SIGTERM,
or TERM for short, which will send a signal to the specified process
requesting that it terminate immediately.  In this case, I'm going to
send a TERM signal to a process by typing an uppercase `S`.  Following
the prompts, I can type in a signal name such as HUP for hang up, KILL
for kill or TERM for terminate, among many others.  In this case the
TERM signal is the default, so I'll just press the Enter key.  Now `top`
is asking for a process id (pid), so I'll type one in and press the
Enter key, at which time the TERM signal will be sent to the `pid` that
I specified.  If all goes well, you should see that process be
terminated and it should no longer show up in the `top` output.


### Options can be specified from the command line as well

Keep in mind that `top` can also have options specified upon the command
line, so if I wanted to display the process list sorted by cpu usage,
but update that output every 5 seconds instead of using the default,
I'd use the following command:

    top -s 5 -o cpu

The `-s` option is a flag used for the update interval (in seconds) and
the `-o` flag specifies the primary keyname with which to sort upon.

Now that we've looked at how to view these processes using `ps` and
`top`, it's time to move on to how these processes are created.  One of
the best ways to demonstrate this is to run programs directly from the
command line.


### Run a command in the foreground

Most of the commands and utilities we've worked with so far in this
tutorial have been run as foreground processes.  This isn't easy to
verify because most of the commands run so quickly, the processes are
spawned and die almost instantly, as when we run commands such as `cd`,
`ls` and many others.  However, the idea of foreground processes becomes
more apparent and important when we have a long-running process.

Revisiting the `find` command from a previous chapter, let's search for
all files under the root directory with a name containing the
string `.plist`:

    find / -name "*.plist"

But what happens if you realize that the command you just entered isn't
exactly what you wanted?  One option would be to wait for that process
to finish and then type in what you really need.  However, this would
cause you an unnecessary delay, and for some processes it might take
much longer than the `find` command just entered.

Instead, it would be really nice if there was an easy way to terminate
a process, so let's do that.


### Terminate a process with Ctrl-c

Whenever you have a command that you need to terminate, you can do this
by typing `Ctrl-c`.

    /Applications/GarageBand.app/Contents/Resources/garageband.help/Contents/Info.plist
    /Applications/GarageBand.app/Contents/Resources/garageband.help/Contents/version.plist
    /Applications/GarageBand.app/Contents/Resources/StoreInit.plist
    ^C

After typing that keystroke combination, you can see it shows up on the
screen and the process has indeed been terminated.  This is a really
common technique, so be sure to remember it since you'll probably use it
often.


### Suspend a process with Ctrl-z

As an alternative to terminating a process, it's sometimes nice to allow
the process to keep running, but just suspend it temporarily.  This is
easily done by typing `Ctrl-z`.  I'm going to run that `find` command
again so that you can see how to suspend it:

    find / -name "*.plist"

Now it's time to type `Ctrl-z`:

    /Applications/iTunes.app/Contents/Resources/pt.lproj/ColumnWidths.plist
    /Applications/iTunes.app/Contents/Resources/pt.lproj/DefaultTags.plist
    /Applications/iTunes.app/Contents/Resources/pt.lproj/genresLoc.plist
    ^Z
    [1]+  Stopped                 find / -name "*.plist"

In the next to the last line of output, you can see the `^Z` character,
followed by another line which shows the process was stopped, but not
terminated.


### The jobs command

For us to better grasp the concept of foreground and background
processes, we need a way to inspect and manage them.  The `jobs` command
is an excellent way to do that, so let's look use the `help` utility for
this builtin shell command.

    iMac:~ screencast$ help jobs 

    jobs: jobs [-lnprs] [jobspec ...] or jobs -x command [args]
        Lists the active jobs.  The -l option lists process id's in addition
        to the normal information; the -p option lists process id's only.
        If -n is given, only processes that have changed status since the last
        notification are printed.  JOBSPEC restricts output to that job.  The
        -r and -s options restrict output to running and stopped jobs only,
        respectively.  Without options, the status of all active jobs is
        printed.  If -x is given, COMMAND is run after all job specifications
        that appear in ARGS have been replaced with the process ID of that job's
        process group leader.

Let's now look at the jobs we have running on our system:

    iMac:~ screencast$ jobs
    [1]+  Stopped                 find / -name "*.plist"

We can see that the output shows the `JOBSPEC`, followed by a process
status that indicates this job is currently stopped, and finally the
full command is shown that this job represents.

Alternatively, if in addition to the normal jobs output, you'd like to
see the process id's associated with each job you're running, just pass
the `-l` option:

    iMac:~ screencast$ jobs -l
    [1]+  1662 Suspended: 18           find / -name "*.plist"

If you're interested in only the process id's, just pass the `-p`
option:

    iMac:~ screencast$ jobs -p
    1662


### Run a command in the background with &

Let's start over by running the `find` command, except this time let's
redirect standard output (stdout) and standard error (stderr) to a file
named `plist.out`.

We're also going to run this command in the background, which can be
done by appending our command with an ampersand symbol `&`:

    iMac:~ screencast$ find / -name "*.plist" > plist.out 2>&1 &
    [1] 58977

Running a command as a background process allows us to continue with our
work.  So let's do that by inspecting the processes that we're currently
running.


### Send a running process to the background with bg

Let's say that you run a command and forget to type the ampersand symbol
`&` to run the program as a background process.  The good news is that
there's an alternative approach that can be used.

The `bg` command is a shell builtin that can take a foreground process
and send it to the background.  Let's take a look at the help for this
command first:

    iMac:~ screencast$ help bg
    bg: bg [job_spec ...]
        Place each JOB_SPEC in the background, as if it had been started with
        `&'.  If JOB_SPEC is not present, the shell's notion of the current
        job is used.

So let's give this a try.  We'll run that `find` command again and
then suspend it using the `Ctrl-z` trick we learned earlier:

    iMac:~ screencast$ find / -name "*.plist" > plist.out 2>&1
    ^Z
    [1]+  Stopped                 find / -name "*.plist" > plist.out 2>&1

Taking a look at the output, we can see the JOB_SPEC was reported within
square brackets when the process was suspended, so let's use that with
`bg`:

    iMac:~ screencast$ bg 1
    [1]+ find / -name "*.plist" > plist.out 2>&1 &

Now let's take a brief look at the process again using the `jobs`
command:

    iMac:~ screencast$ jobs
    [1]+  Running                 find / -name "*.plist" > plist.out 2>&1 &

And we can see that it's now running again.

You can continue doing work on the command line and once the process has
completed, you'll see the status sent to standard output indicates that
it has indeed finished:

    iMac:~ screencast$ 
    [1]+  Exit 1                  find / -name "*.plist" > plist.out 2>&1


### Bring a background process to the foreground: fg

To complement the ability to send processes to the background, we also
have the ability to bring processes to the foreground, using the `fg`
shell builtin.  Let's view the help documentation for this command to
get more details:

    iMac:~ screencast$ help fg
    fg: fg [job_spec]
        Place JOB_SPEC in the foreground, and make it the current job.  If
        JOB_SPEC is not present, the shell's notion of the current job is
        used.

So let's take a look at an example of this command.  I'll revisit the
`find` command we used earlier, but this time I won't redirect standard
output or standard error, just so you can see what's happening more
clearly:

    iMac:~ screencast$ find / -name "*.plist" 
    find: /.DocumentRevisions-V100: Permission denied
    find: /.fseventsd: Permission denied
    find: /.Spotlight-V100: Permission denied
    find: /.Trashes: Permission denied
    /Applications/Aimersoft DVD
    Ripper.app/Contents/Frameworks/CodecPlatform.framework/Versions/A/Resources/Info.plist
    /Applications/Aimersoft DVD
    Ripper.app/Contents/Frameworks/ConvertManager.framework/Versions/A/Resources/Info.plist
    /Applications/Aimersoft DVD
    Ripper.app/Contents/Frameworks/RegisterDlg.framework/Versions/A/Resources/Info.plist
    ^Z
    [1]+  Stopped                 find / -name "*.plist"

First, I run the command and immediately suspend it using `Ctrl-z`.
I can see from the output that the process is stopped and that it's
JOB_SPEC is 1.

Let's use the `fg` command to bring it to the foreground:

    iMac:~ screencast$ fg 1
    find / -name "*.plist"
    /Applications/GarageBand.app/Contents/Resources/StoreInit.plist
    /Applications/GarageBand.app/Contents/version.plist
    /Applications/GitX.app/Contents/Frameworks/Sparkle.framework/Resources/Info.plist
    /Applications/GitX.app/Contents/Frameworks/Sparkle.framework/Resources/SUModelTranslation.plist
    .
    .
    .
    (output truncated)

As you can see, the program runs in the controlling terminal in the
foreground, sending standard output and standard error for us to view.

Although that's a good trick to know, I think this approach really
shines in my day-to-day work flow when I'm writing code or working on
describing a technical tip.  I usually work in `vim`, so let's take
a quick look at how this would work.

First, I would open a file in `vim`, do some work and then save it.
Occasionally I might prefer to drop out to a shell to run a command, but
I don't want to go through the hassle of quitting `vim`, especially when
I might have several files open at one time.  It would be much easier if
I could just suspend my `vim` session to enter a shell command and then
quickly return.  We've already seen how to suspend a process using
`Ctrl-z`, and also how to bring a process to the foreground using `fg`,
so let's try it out:

    iMac:~ screencast$ vi plist.out 

I'll open the **plist.out** file that we created earlier.  Then, while
within `vim`, if I decide to drop out to a shell, I can just type
`Ctrl-z`, and I'll see that the process has been suspended:

    [1]+  Stopped                 vi plist.out

Then I can do whatever I need, such as change a file permission, look at
my code repository status in `git`, or run other commands.

When I'm ready to return to my suspended session, I'll I need to do is
issue the `fg` command:

    iMac:~ screencast$ fg
    vi plist.out

And as you can see, I'm back within `vim` and can continue my work
there.


## Killing processes

Having all of these processes is great, but sometimes you need a way to
destroy a process.  Maybe you typed the wrong command, or there's
a process that is eating a huge amount of memory or consuming too many
cpu cycles.  Regardless of the scenario, you need to prevent the process
from continuing.  This can be done using the `kill` command.

The `kill` command is really just a facility for sending a signal to
a process.  Using the `-l` option, we can view a list of all signals
that can be sent:

    iMac:~ screencast$ kill -l
     1) SIGHUP   2) SIGINT   3) SIGQUIT  4) SIGILL
     5) SIGTRAP  6) SIGABRT  7) SIGEMT   8) SIGFPE
     9) SIGKILL 10) SIGBUS  11) SIGSEGV 12) SIGSYS
    13) SIGPIPE 14) SIGALRM 15) SIGTERM 16) SIGURG
    17) SIGSTOP 18) SIGTSTP 19) SIGCONT 20) SIGCHLD
    21) SIGTTIN 22) SIGTTOU 23) SIGIO 24) SIGXCPU
    25) SIGXFSZ 26) SIGVTALRM 27) SIGPROF 28) SIGWINCH
    29) SIGINFO 30) SIGUSR1 31) SIGUSR2

To send a signal to a process, we'll use the `kill` command, followed by
an optional signal and finally the process id, or `pid`, that we'd like to
communicate with.  By default, the TERM signal is sent to the process id
if one isn't specified.

So let's run that trusty `find` command again and immediately terminate
it using `ctrl-z`:

    iMac:~ screencast$ find / -name "*.plist" 
    find: /.DocumentRevisions-V100: Permission denied
    find: /.fseventsd: Permission denied
    ^Z
    [1]+  Stopped                 find / -name "*.plist"

Now we can use `ps` to view the running processes for the current user:

    iMac:~ screencast$ ps 
      PID TTY           TIME CMD
      611 ttys002    0:00.06 -bash
    43060 ttys002    0:00.69 find / -name *.plist
      616 ttys003    0:00.01 -bash
    39263 ttys003    0:37.54 vi 08_working_with_processes.md

And you can see, the `find` command has a process id of 43060.  Let's
terminate it with `kill` and its process id:

    iMac:~ screencast$ kill 43060
    iMac:~ screencast$ 
    [1]+  Terminated: 15          find / -name "*.plist"

A little while later we receive the response that the process was in
fact terminated, which is exactly what we wanted.

As an alternative, we can pass the JOBSPEC instead of the process id to
kill the process:

    iMac:~ screencast$ find / -name "*.plist" 
    find: /.DocumentRevisions-V100: Permission denied
    find: /.fseventsd: Permission denied
    [1]+  Stopped                 find / -name "*.plist"

Again, we'll run the `find` command and suspend it using `Ctrl-z`.  But
this time, we'll skip using `ps` to view the running processes because
the output from suspending the process indicates the JOBSPEC of 1, so we
can terminate it by passing that number to `kill` instead:

    iMac:~ screencast$ kill %1
    [1]+  Stopped                 find / -name "*.plist"

    Side note: prepend the job spec with %

I'll now run the process status again:

    iMac:~ screencast$ ps
      PID TTY           TIME CMD
      611 ttys002    0:00.06 -bash
      616 ttys003    0:00.01 -bash
    39263 ttys003    0:37.54 vi 08_working_with_processes.md
    [1]+  Terminated: 15          find / -name "*.plist"

Now we can see the process has in fact been terminated.

But what if we need to send a signal other than TERM?  For example, it's
quite common for me to send the KILL signal to terminate a process,
which is a non-ignorable signal.

Let's do that.  So, we'll run the `find` command again and issue
a `Ctrl-z`:

    iMac:~ screencast$ find / -name "*.plist" 
    find: /.DocumentRevisions-V100: Permission denied
    find: /.fseventsd: Permission denied
    ^Z
    [1]+  Stopped                 find / -name "*.plist"

Now we can send a KILL signal, which can be done using the signal names,
such as:

    kill -SIGKILL pid

Which can also be abbreviated by removing the `SIG` portion of the name:

    kill -KILL pid

We can also replace the process id with a JOBSPEC if we want.

Please notice that the dash that prepends the signal name or number is
important.  If you leave it off, you'll get an error:

    iMac:~ screencast$ kill SIGKILL %1
    -bash: kill: SIGKILL: arguments must be process or job IDs
    -bash: kill: %1: no such job

Returning to our example, because sending a KILL signal is so common,
it's easier to pass the signal number for KILL, which is a number 9:

    iMac:~ screencast$ kill -9 %1
    [1]+  Stopped                 find / -name "\*.plist"

Because I've used the KILL signal so many times, remembering that a 9 is
easy.  However, most of the other signals I have not committed to
memory, so referring to the signal list is sometimes necessary.

Let's revisit that list one more time as a refresher:

    iMac:~ screencast$ kill -l
     1) SIGHUP   2) SIGINT   3) SIGQUIT  4) SIGILL
     5) SIGTRAP  6) SIGABRT  7) SIGEMT   8) SIGFPE
     9) SIGKILL 10) SIGBUS  11) SIGSEGV 12) SIGSYS
    13) SIGPIPE 14) SIGALRM 15) SIGTERM 16) SIGURG
    17) SIGSTOP 18) SIGTSTP 19) SIGCONT 20) SIGCHLD
    21) SIGTTIN 22) SIGTTOU 23) SIGIO 24) SIGXCPU
    25) SIGXFSZ 26) SIGVTALRM 27) SIGPROF 28) SIGWINCH
    29) SIGINFO 30) SIGUSR1 31) SIGUSR2 


## Chapter summary

So, that wraps up this chapter on processes.  We've seen how to view
processes, how to send processes to the background and also bring them
to the foreground, how to send signals to those processes and a number
of other handy tricks.

Just remember that processes are a core feature of the UNIX operating
system, so having this knowledge is essential to understanding how UNIX
works and increasing your knowledge about how to get the most out of
your system.
