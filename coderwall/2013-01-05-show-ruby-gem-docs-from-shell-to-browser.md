#Show Ruby gem docs from shell to browser

### Background

Let's say I'm working on a **Ruby** library or **Rails** related project and I'm interested in learning more about a
particular Ruby gem, but I'd prefer to read the
documentation using a web browser.

### A starter example

I'll run this little `gem` command to find out more about pdf-reader:

    gem list pdf-reader -d

And here is the output...

    *** LOCAL GEMS ***

    pdf-reader (1.3.0, 1.2.0, 1.1.1)
    Author: James Healy
    Homepage: http://github.com/yob/pdf-reader
    Installed at
    (1.3.0): /Users/chip/.rvm/gems/ruby-1.9.3-p194
    (1.2.0): /Users/chip/.rvm/gems/ruby-1.9.3-p194
    (1.1.1): /Users/chip/.rvm/gems/ruby-1.9.3-p194

    A library for accessing the content of PDF files


Basically, this tells `gem` to **display gems whose name starts with the string** `pdf-reader`, and by using the `-d` option, it asks the results to **display detailed information of the gem(s)**.

## Full example

Since most gem authors include a Homepage for their documentation,
using this tiny mixture of `UNIX` commands will do the trick (line
continuation character used only for formatting):

    gem list pdf-reader -d | \
    grep Home | \
    awk '{ print $2 }' | \
    xargs open
    
What this command sequence does is take the output previously shown and only display the line containing the string `Home`, like so:

    Homepage: http://github.com/yob/pdf-reader
    
The `awk` command then takes the second string, which is the url provided for the gem's Homepage, and passes it to `xargs` command, which in turn takes the url as an **argument** and passes it to the `open` command. Ultimately, the command **opens a file (or a directory or URL), just as if you had
     double-clicked the file's icon**.  In this case, it simply opens the `gem` documentation in a web browser.
     
This series of commands could easily be converted to a shell `function` or `alias` to make it easier to use. One caviat is that not all `gem` authors provide a `Homepage` url in their `.gemspec`, so please keep that in mind when using this.
