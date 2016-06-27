---
title: Install and Initialise Hexo
date: 2016-06-23
categories: Hexo
tags: ['Starting Up','Blogging','Hexo']
---

Inspired by the awesome **[Pluralsight](https://www.pluralsight.com/ "Pluralsight")** and it's course on [static site generators](http://www.staticgen.com/ "Title"), I decided to implement a blog using hexo.

The steps shown below are what you need to install and initalise Hexo locally.

## Required Applications and Installations

The following items were required to create my blog, which I either already had or required to install.

* **[Git](https://git-scm.com/downloads "Git")**  - Of course I already had it installed! Git will provide the methods of source control and also in my case, provide the host for my blog web page.
* **[Node.js](https://nodejs.org/en/ "Node.js")** - Downloaded this for windows X64, as I have a 64 bit machine, chose all the defaults and so "Next Next Nexted" until Finish.
* **NPM** - This installed with the latest version of Node.js above anyway so I didn't need to do anything here.
* **[Atom](https://atom.io/ "Atom")**  - Even though I'd used notepad++ as my text editor up until this point, it didn't have any native markdown features so I got converted to atom. It has appropriate syntax highlighting for markdown and has a useful live HTML preview feature (CTRL+Shift+M). Therefore my first blog post (this one) was written with atom and I'm likely to stick with it...
* **[Hexo](https://hexo.io/ "Hexo")** - Is a static site generator application and the main application I have used to implement my blog. The home page of Hexo gives you the command to install, so I opened up my command prompt and typed it in. At the time of my implementation of my blog this was...
```
npm install hexo-cli -g
```
**Note:** I actually used the node.js command prompt when performing these tasks, not windows command prompt, in case windows command prompt doesn't work!

Then type in hexo into the command prompt and if the installation was a success you'll get a load of hexo details back in the command window. Tidy!

## Using Hexo to Initiate my Folders and Files
I created a folder in the desired location for my dataGriff blog by typing in the command (replace [My Path] with your folder path)
```
mrkdir //[My Path]/dataGriff
```
Then I changed directory to this folder using
```
cd //[My Path]/dataGriff
```
Finally I initialised hexo on the folder using
```
hexo init
```
There was a little message at the end of the command prompt output that stated
```
INFO Start blogging with hexo
```
So I thought I must be on the right track!

![HexoStartBlogging](/images/HexoStartBlogging.png)))

## Viewing Hexos Startup Template
In command prompt, in my dataGriff directory, I typed
```
hexo server
```
Again I got the satisfying message of
```
INFO hexo is running at http://localhost:4000/.
```
So with trepidation I went to the http://localhost:4000/ link in a browser and...
Hey presto! I could see hexos startup template! Success!
To stop the page running you can just press CTRL+C in the command prompt and confirm the termination.

![HexoDefault](/images/HexoDefault.png)
