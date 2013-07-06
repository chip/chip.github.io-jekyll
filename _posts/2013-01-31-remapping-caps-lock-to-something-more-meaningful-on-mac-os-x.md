---
layout: post
title: "Remapping Caps Lock key to something more natural on Mac OS X"
description: ""
category: 
tags: [caps capslock ctrl control mac osx modifier key keys keyboard keystroke keymapping qwerty]
---
{% include JB/setup %}

Over the years I've tried to continually improve my ability to use the keyboard
for navigating around my system.  I've used a wide variety of systems throughout
my career that spans nearly two decades, and for the last several years have
focused on Mac OS X for my desktop work.

As most Mac addicts are aware of, there are numerous keyboard shortcuts
available that will enable them to speed up their workflow by not having to lift
their hand and make use of a mouse or trackpad.

These keystroke combinations generally require heavy use of *modifier keys*,
which include specific keys such as `Command`, `Shift`, `Option`, `Control`,
`Caps Lock`, and the `Fn` key.

As a software developer, I find that I need some of these modifier keys
extremely often, but one key that I use quite frequently is the `Control` key.
However, on my QWERTY keyboard, I personally find that the `Control` key is
located in a very awkward place.  For years I struggled with using this key, but
due to its placement, I very rarely would find it properly.  Even when I did, it
often required me to slow down quite a bit, which I found really interrupted my
flow, even though I'm not an expert typist at roughly 59 words per minute.
I learned to type in high school, so at this point I have over a quarter century
of practice.  In my view, I should be a much faster typist, so it's going to
take a good bit of work to improve my speed.  One way to do that aside from
regular practice sessions on sites such as
[typeracer](http://play.typeracer.com/) and the like, is to remap certain keys
to a more natural location on the keyboard.

**My suggestion: Remap the `Control` key!**

"But which key do I map it to?", you ask?  Well, it just so happens that
I almost never need the `Caps Lock` key.  Most capitalization of words occurs
for a single character, but in those cases I think the `Shift` key is perfectly
sufficient for accomplishing that goal.  Also, I think the `Caps Lock` key is in
a very easy-to-reach location on the keyboard, and given that I use the
`Control` key quite a lot, this should be a perfect fit.

So how do you achieve remapping a key on OS X?  Let me show you...

Here are the instructions:

1.  Open **System Preferences** on the Mac.
    I invoke **Spotlight** by clicking the `Command` and the `spacebar` at the
    same time, then typing **System Preferences**.

    ![Spotlight](/assets/spotlight.png)

2.  Select **Keyboard** from the System Preferences.

    ![Keyboard](/assets/keyboard.png)

3.  From the **Keyboard** panel click the **Modifier Keys...** button.
    Do **NOT** select the **Keyboard Shortcuts** panel.

    ![Modifier Keys](/assets/modifier%20keys.png)

4.  Select **^ Control** from the menu to the right of the **Caps Lock Key**
    setting, then click the **OK** button.

    ![Caps Lock mapping](/assets/caps%20lock%20mapping.png)

That should complete the key remapping.  

Now, anytime you need to do an operation that requires the control key, such as
suspending a UNIX process with **Ctrl-Z**, simply press the `Caps Lock` key
instead.  I have used this key remapping for over a year and found that it has
greatly improved my work flow.  Give it a try and see if you get the same
results.
