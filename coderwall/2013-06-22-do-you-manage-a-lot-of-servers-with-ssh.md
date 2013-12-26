# Do you manage a lot of servers with ssh?

If you have a lot of servers that you manage and you can't remember all of the
IP addresses, add a configuration like this to `~/.ssh/config`:

    Host macbook
    HostName 192.168.1.50
    User chip

Substitute the values for **Host**, **HostName** and **User** to suit your specific
needs.

Logging in is now as easy as this:

    ssh macbook
    
This is basically equivalent to running this command:

    ssh chip@192.168.1.50
    
For me though, the former example is much easier to remember, so I've always found this shortcut to come in handy.  I hope you find it useful as well.
