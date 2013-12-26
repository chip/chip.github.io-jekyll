# Vim can autocorrect misspellings and typos

Do you use `vim`?

Well, if so, do you misspell or typo words on a regular basis?

For example, maybe you type **teh** when you intended to type **the**?

Well, `vim` has a build-in feature for handling this, so just add this to your
`.vimrc`:

    iabbrev teh the

This tells vim to use the **iabbrev** feature to automatically correct the word
**teh** with the corrected form of the word **the**.

One downside of this approach is adding a lot of these definitions can clutter
up your `.vimrc` file.  

As an alternative, there is a little `vim` plugin I created called
**[vim-fat-finger](https://github.com/chip/vim-fat-finger)** which supports over
4,000 common misspellings and typos.  I hope you find it useful.
