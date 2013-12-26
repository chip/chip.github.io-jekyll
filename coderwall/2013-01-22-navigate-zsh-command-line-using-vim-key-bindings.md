#Navigate the zsh command line using vim key bindings

If you use the `zsh` shell, you can easily edit the command line using `vim` key bindings.
Just add this to your `~/.zshrc`, like so:

    bindkey -v

Be sure to "source" the shell initialization afterwards with:

    source ~/.zshrc

Here's another shortcut for accomplishing the same sourcing:

    . ~/.zshrc

Keep in mind that you'll be in **insert mode** as you type, so you'll need to enter **normal mode** using the **ESC** key to navigate , just like in `vim`.  Now you can navigate the command line using vim key bindings such as typing zero `0` to go to the beginning of the line, typing a dollar sign `$` to reach the end of the line, and so on.  Just like in `vim`, you'll need to switch back to **insert  mode** to start typing again.

If you're a regular `vim` and `zsh` user, be sure to give this a try, as it will be a 
huge time-saver.
