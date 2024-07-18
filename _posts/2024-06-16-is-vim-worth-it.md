---
layout: post
title: Is Vim and Emacs worth it?
date: 2024-06-16 
tags: general opinion
categories: code
author: Sidhin S Thomas
published: false
---

Like any other dev out there. I too enjoy a good set of tools. Especially ones that boost productivity. So as a vscode user I am constantly 
bombarded with content talking about vim/ emacs/ neovim and how being keyboard only is a huge productivity boost.

In general the arguements I have heard boils down to these - 
* Not having to use mouse is truely effecient.
* VsCode env and IDEs are very bloated.
* Customizable - infact looks like folks have even set up emacs as their desktop environment - impressive.
* It's pretty damn cool!

Very compelling, and it does look cool seeing all the commandline jutsu I see people do in office. So naturally I too succumb
to peer pressure, and decided decided. You know what, I also want to be a haxxor.

## Choosing Vim(NeoVim) over Emacs

Nothing against Emacs, it's a pretty great software. But it also has a high barrier of entry. I was already kind of familiar with Vim due to having to 
interact with it in term/ ssh etc, so decided to use NeoVim to start on my KB Code Factory journey.

Now installing and getting started isn't too hard. I use two platforms mainly - windows for work and mac at home. So installing neovim in 
both is pretty simple.

```
# Windows
winget install Neovim.Neovim
# Mac
sudo port install neovim
```

This is where it starts. I tried the vimtutor which describes the basics of VIM. The different modes, how you do all sorts of text editor operations. like copy, move word to word, search, edit just a character etc. Loved the tutorial.

It just simply made sense! There are all sorts of nifty movement mechanisms. Fine-tuned search replace. Deleting lines, yank, replace, clear etc. All of them
very fun to use. And makes you feel so productive.

But then I had to customize the editor. Install plugins etc. After reading about it a bit. I realized it's too much effort to spend right now so decided to install Vim extension in Vscode. Should get most of advantages there.

### Vscode Vim mode

This was pretty good, I get to experience Vim motions in my already configured to perfection editor. Over than the usual Vim keybindings I found the following very useful - 

-  Ctrl + ` for opening terminal
- `ctrl + P` for file switching and commands (Type ">")
- `ctrl + shift + E` for file explorer - though I'll be honest, mouse is faster than keyboard for navigating it. Since I can't prefix the path by typing.
- `ctrl + B `- to close side bar
- `ctrl + shift + B` Secondary bar where my copilot chat is present.



