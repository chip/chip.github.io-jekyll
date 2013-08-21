---
layout: post
title: "Using the tree command in UNIX"
description: ""
category: 
tags: [unix tree ls vim git vim-gitgutter]
---
{% include JB/setup %}

# Using the tree command in UNIX

The `tree` command is a handy alternative to `ls` which list the contents of
directories in a tree-like format.  In the examples that follow, I'm going to
show the contents of a plugin for [vim](http://www.vim.org/) called
[vim-gitgutter](https://github.com/airblade/vim-gitgutter), which I find to be
very helpful when I work with git repositories.

Let's look at a few options that the `tree` command offers.


### Basic usage

By default, the `tree` command displays output based on the current working
directory.

    ☺ tree
    .
    ├── README.mkd
    ├── doc
    │   ├── gitgutter.txt
    │   └── tags
    ├── plugin
    │   └── gitgutter.vim
    └── screenshot.png

    2 directories, 5 files

Alternatively, you can specify a directory argument as well:

    ☺ tree ~/.vim/bundle/vim-gitgutter 
    /Users/chip/.vim/bundle/vim-gitgutter
    ├── README.mkd
    ├── doc
    │   ├── gitgutter.txt
    │   └── tags
    ├── plugin
    │   └── gitgutter.vim
    └── screenshot.png

    2 directories, 5 files


### List directories only

    ☺ tree -d
    .
    ├── doc
    └── plugin

    2 directories


### Show human readable file sizes

    ☺ tree -h
    .
    ├── [9.6K]  README.mkd
    ├── [ 136]  doc
    │   ├── [7.5K]  gitgutter.txt
    │   └── [1.0K]  tags
    ├── [ 102]  plugin
    │   └── [ 16K]  gitgutter.vim
    └── [ 16K]  screenshot.png

    2 directories, 5 files


### Display file permissions

    ☺ tree -p
    .
    ├── [-rw-r--r--]  README.mkd
    ├── [drwxr-xr-x]  doc
    │   ├── [-rw-r--r--]  gitgutter.txt
    │   └── [-rw-r--r--]  tags
    ├── [drwxr-xr-x]  plugin
    │   └── [-rw-r--r--]  gitgutter.vim
    └── [-rw-r--r--]  screenshot.png

    2 directories, 5 files
