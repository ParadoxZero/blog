---
layout: post
title: Is Vim and Emacs worth it?
date: 2025-03-16 
tags: general opinion
categories: code
author: Sidhin S Thomas
published: true
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

Unfortunately, vscode workflows at work required mouse anyways, so I found vim motions to be conflicting with using vscode overall. So decided to go to neovim again.

## Back to NeoVim

I went back to neovim again and had a bunch of iterations after understanding the plugin system. Started with kickstart and then crafted my own config. All stored in the github repo - `https://github.com/ParadoxZero/nvim-config.git`.

Honestly, I have enjoyed this little piece of sofware a lot. It looks pretty, vim motions are fun, I guess especially when you do a lot of text editing. 

I loved the "feel" of Neo Vim. Loved lazygit integration via terminal. Easy lsp management. Kodos to the neovim community for making this all so seemless and easy to use.

However at the end of the day, I couldn't continue using neovim

### Performance 
The biggest reason for switching back was perf. I am not sure why but with clangd on - neovim absolutely dies when I am working on chromium. There is simply no way I could have any reasonable productivity with it. 

Another thing was startup time. Due to the heavy build process, I close my editor often to avoid lsp and other stuff consuming disk and cpu resources. So startup time is very important to me. With even minimum amount of plugins, my neovim always took a lot longer to startup than my vscode - I couldn't wrap my head around why, but that's what is it.

### Usage
Another big reason I couldn't continue with vscode was that I realized I don't do a lot of actual "Editing". 

My workflow generally is code research, debugging, and only little bit of addition. Very rarely am I writing full blown 
new classes and modules. Maybe 5 times a year. 

For most part I am intersted in making small changes in large amount of files that can be in arbitary locations accorss code bases. I realized for these things I really like the traditional GUI + Keybinding workflow used in vscode vs keyboard heavy one. For example, scolling around code without knowling a keyword is a big thing I do, which totally defeats the keyword based file movement in vim. I can defintely use mouse scroll in nvim too, but that then defeats the purpose. 

In fact the mode based editing actively hinders this workflow where I scroll the file searching for a pattern that I can't search for, seeing the relavent spot, I'll then add or delete some code by placing curor on the correct spot. 

In nvim, I have an extra step of pressing i or a to enter the edit mode which becomes frustuating very quickly. 

## Conclusion

Overall I enjoyed the nvim experience but for my day to day workflow it's too incompatible and hence I decided to go back to vscode which offers the 
right balance of performance, customization, features and ease of use.

I still miss telescope way of searching through the active file though.