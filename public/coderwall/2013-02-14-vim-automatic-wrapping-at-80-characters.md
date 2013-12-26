#Vim automatic wrapping at 80 characters

If you want to force `vim` to always automatically wrap lines when it hits the 80
character mark, I would suggest doing the following:

First, edit your vim resource file:

    vim ~/.vimrc

Next, set the option for **textwidth** to the number of characters limit:

    set textwidth=80
    
Here's an alternative *shorthand version* of the textwidth option:

    set tw=80

Last, set the option for **formatoptions**:

    set formatoptions+=t

Here's an alternative *shorthand version* of the formatoptions option:

    set fo+=t

Now vim will automatically wrap your lines once you reach the 80 character limit, which I find to really help me keep my code clean.  This really comes in handy if you're on a team the does a ton of code review using **Github Pull Requests** as I do, especially since you don't have to scroll horizontically, which would break up your flow of reading.

