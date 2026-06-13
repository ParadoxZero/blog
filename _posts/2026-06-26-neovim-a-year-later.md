---
title: NeoVim a year later
published:
tags:
  - code
  - tools
date: 2026-06-13
categories: code
author: Sidhin S Thomas
layout: post
---
Last year I did an experiment trying out Vim/ Vim Motions as my primary editor and concluded that it's not for me. 

Today, I am writing this post to document how that opinion has evolved.

## Primary Gripe from last year

"It was slow"

That was the biggest and most major issue. Which I realized was due to my running these tools in windows. Now
Windows is a great OS. I love working on it. But it's not the first preference for the OSS devs who build these tools.

As a consequence, non corporate backed Open Source Software generally suffers in their experience when running
them in windows OS.

## What changed

I changed my job.

Moving from Microsoft to a Linux based shop changed my dev tools and environment. Suddenly I was using a blade
server instead of a physical desktop to code.

Which meant, command-line utils and tools suddenly became the first class citizen in my workflows. Which in turn
prompted me to try out neovim again due to it's plugin ecosystem.

>**Note** - I used vscode ssh plugin for a bit. I just felt keeping things inside a tmux session worked better for me.

## How is it going?

Well I now have an elaborate neovim config. I have vim motions enabled in everything that gives me the option. And 
I feel handicapped when I am not able to press esc and move around and edit things using the motions. It has become
a second nature to me.

Getting used to took way less time than I thought. I was automatically using the basic motions 2-3 days into the switch.
Having quick help from AI was a tremendous boost. Anytime I feel that something probably can be done efficiently using
vim motions, I just asked AI. And boom, more often than not, there indeed was a quick key-bind to get that done.

In fact, I loved the keyboard based workflow so much that I have moved to a tiling window manager - sway in ubuntu and
glazewm in windows.