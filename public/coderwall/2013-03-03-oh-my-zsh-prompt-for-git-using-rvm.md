# Oh-my-zsh prompt for git (using RVM)

Now that I've been using `zsh` as my primary shell for my iMac for quite some
time, I've found that I was long overdue for pimping out my prompt.

After installing Robby Russel's amazing
**[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)**, I decided to
customize my own theme.  So here's a copy of it:

    user="%n"
    host="%m"
    directory="%~"
    local smiley="%(?,%{$fg[green]%}☺ %{$reset_color%},%{$fg[red]%}☹ %{$reset_color%})"

    PROMPT=$'$fg[white]$user@$host%f $fg[red]$directory%f $fg[white]($(rvm-prompt))%f $(git_prompt_info)\n$smiley'

    ZSH_THEME_GIT_PROMPT_PREFIX="$fg[red]["
    ZSH_THEME_GIT_PROMPT_SUFFIX="]"
    ZSH_THEME_GIT_PROMPT_DIRTY=" ●"
    ZSH_THEME_GIT_PROMPT_CLEAN=""

Starting from the top, let's describe what's happening here per the [zsh
documentation](http://zsh.sourceforge.net/Doc/Release/Prompt-Expansion.html).

First, the `user` is set to `%n`, which is a shortcut for `$USERNAME`.

Next, the `host` is set to `%m`, which is a shortcut for the hostname up to the first `.`. 

Then the `directory` is set to `%~`, which if the current working directory
starts with `$HOME`, that part is replaced by a ‘~’.

The `smiley` is much more complicated, so let's break it down into smaller
chunks and describe each part individually.

First, the beginning of the line has `%(?,`, which will return status of the
last command executed just before the prompt. If it returns true, it will show a
green smiley `☺`, otherwise a red frown `☹`.

This is done by using various functions provided by
**[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)**, by setting the
foreground color to green or red before printing the smiley, and then resetting
the color back to its original setting.

Moving down to the `PROMPT` setting, we see that the following occurs:

1. Set the foreground color to white and display `$user@$host`
2. Set the foreground color to red and display `$directory`
3. Set the foreground color to white again and display the
   [rvm-prompt](http://rvm.io/workflow/prompt) provided by RVM.
4. Display the git prompt which is also provided by
   **[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)**.
5. Print a newline and display the contents of our `smiley` variable, which
   will be red or green depending on the result of the last command.

The remaining variables starting with `ZSH_THEME_GIT_PROMPT` are only related
to the git status. **The important part is the command checks the git status to
see if it is clean or not.**  If so, it shows the red dot to indicate that case,
which for example could be caused by having changed uncommitted files, or files that have
been added to the git index but not yet committed.  **By having that visual indicator, you can quickly see when your repository is in a dirty state.**

I think customizing your shell prompt is a great way to pack a lot of
information about your current environment and be able to view it at a quick
glance, allowing your workflow to maintain a swift pace.  I hope this benefits
you as much as it has for me.

