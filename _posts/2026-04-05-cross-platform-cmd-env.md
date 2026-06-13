---
layout: post
title: Cross platform env
date: 2026-06-13
tags: code cross-platform
categories: code
author: Sidhin S Thomas
published: true
---

One of the most tiresome thing I have to do when I set up a new machine is configuring my environment. 
Especially if that's a dev box, which is all of my machines, thanks to my "hobby" of coding things up.

On a high level these are the things you do - 
* Get your favorite browser (I really hope that's Microsoft Edge!) Or at least chromium.
* Login to all sorts of important accounts - Microsoft, Google, Github, Instagram, whatnot.
* Install your base programs - vscode, android studio, python, rustup etc
* Configure your alias and shortcuts

As a person always seeking to optimize things. I am looking for a single action to complete the
setup. And as such, I might have stumbled onto something. I am here to share the approach which
might be very useful to build easy to use - 
**single script to configure and setup your env**.

## The concept of sym links
Symbolic links are going to be the foundation of the idea. They provide an easy way to virtually
place files from a central location to relevant location. 

That is, the file will exist in the repo folder - say `~/ws/config` or `c:\users\sidhin\ws\config`.
Then a sym link will be created to say `~/.nvim/config/init.lua` or `~/.nu/config.nu`. This allows
for free sync and backup of configs accoss VMs and devices.

This becomes super relevant once you start using cloud dev machines which you might have multiple 
of.

> **Note** - In windows, you'll need to enable developer mode to get symlinks as a feature

## Software and Installation

Now as of writing this post, my configs and scripts are mostly focused around the dev tools and syncing the configs for everything I care about.
The tools I am mostly interested in are -
1. NeoVim
2. Rustup
3. Lazygit
4. fzf
5. ripgrep
6. nushell

There are some more, but these pretty much bootstrap my environment well.

## The repository

Link - https://github.com/ParadoxZero/configs

This repo contains my configs. It has two parts to it - 
1. Python script to detect platform, install software and create symlinks
2. The actual configurations

Using it is very simply. Simply clone the repo then - 
```bash
cd configs
python3 configure.py
# or
python3 configure.py --skip-deps # if you only want to recreate symlinks
```

That's it. The script will take care of the rest.

Currently supports Windows, Mac and Ubuntu.