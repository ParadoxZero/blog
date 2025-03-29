---
layout: post
title:  "Cross platform commandline env"
date:   2025-03-16
tags: code cross-platform
categories: code
author: Sidhin S Thomas
published: false
---

One of the most tiresome thing I have to do when I set up a new machine is configuring my 
envirement. Especially if that's a dev box, which is all of my machines, thanks to my "hobby" of
coding things up.

On a high level these are the things you do - 
* Get your favorite browser (I really hope that's Microsoft Edge!)
* Login to all sorts of important accounts - Microsoft, Google, Github, Instagram, whatnot.
* Install your base programs - vscode, android studio, python, rustup etc
* Configure your alias and shortcuts

As a person always seeking to optimize things. I am looking for a single action to complete the
setup. And as such, I might have stumbled onto something. I am here to share the approach which
might be very useful to build easy to use - 
**single script to configure and setup your env**.

# The concept of sym links
Symbolic links are going to be the foundation of the idea. They provide an easy way to virtually
place files from a central location to relevant location. 

That is, the file will exist in the repo folder - say `~/ws/config` or `c:\users\sidhin\ws\config`.
Then a sym link will be created to say `~/.nvim/config/init.lua` or `~/.nu/config.nu`. This allows
for free sync and backup of configs accoss VMs and devices.

This becomes super relevant once you start using cloud dev machines which you might have multiple 
of.

