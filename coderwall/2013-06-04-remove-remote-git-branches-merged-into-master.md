# Remove remote git branches merged into master

Ok, let's say that you've been working a ton on pushing various topic 
branches using git.  Yeah, it happens.  And later in the day you then move to 
another machine where you don't have those branches stored locally.

It's time to do a little **git housekeeping**.

So, let's get the latest updates:

    git pull --rebase origin master
    git fetch

I normally prefix all of my branches with my name, like `chip/tricked_out_feature`,
just so my branches are all grouped together. At work, I also prepend the 
branch with an id from Pivotal Tracker since that's how we manage the project 
*(as you can see in the example below)*.

This makes finding my branches, easy...

    git branch -r | grep chip

...which will normally return results that look like this:

    origin/chip/41096601_remove_cc_billing_address_id
    origin/chip/48334463_duplicate_extensions

Now, in this case, I know both branches have been merged, so it's easy to 
remove them one at a time.  However, sometimes I have so many branches and 
I'm not sure if certain ones have been merged already.

This command will help me clean up my mess quite nicely *(and avoid trashing*
`master` *in the process)*:

    git branch -r --merged master | \
    grep chip | \
    sed 's/ *origin\///' | \
    grep -v 'master$' | \
    xargs -I% git push origin :%

So what does it do?  Let's walk through it one line at a time.

1. First we check for all of the remote branches by using the `-r` option, then filtering that output with the `--merged` option, which only shows branches **already merged into the master branch**.

2. Next we simply filter the results looking for my branches.

3. Then we perform string replacement using `sed`, which in my opinion is often a neglected command, yet very powerful.  Let's take a look at the unfiltered output so we can understand the command better:

    ```
    origin/chip/41096601_remove_cc_billing_address_id
    origin/chip/48334463_duplicate_extensions
    ```

    So what this call to `sed` does is remove all of the spaces followed by the `origin` and forward slash `/` and replace it with an empty string.

4. The next `grep` command is simply a sanity check, as it removes any lines containing the string `master`.  Based on our previous commands, I doubt that the `master` branch would ever show up in the list of results, but I do this just in case because I generally don't push directly to the `master` branch on team-related projects.

5.  As a final command, we use `xargs` to accept the list of branches and for each input line, and by using the `-I` option we replace the percent sign `%` with the branch name.  Once the string is replaced, the command is prepped and ready to push to the branch, **but it's not a normal push as you might expect**.  Instead, prefixing the branch with a colon `:` tells git to **delete this branch**, so be careful.

This has helped me quickly do housekeeping on branches that I no longer need, so 
it's much easier to glance at my list of active branches.

I've always found that the less clutter I have around, whether it's on my Desktop 
or my git repos, the clearer I think because it doesn't distract me with 
useless garbage.

Hopefully this will work for you as well.

