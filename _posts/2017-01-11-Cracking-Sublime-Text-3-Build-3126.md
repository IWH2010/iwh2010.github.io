---
layout: post
title: Cracking Sublime Text 3 Build 3126
---

Sublime Text is a popular closed-source text editor available on OSX, WIN and Linux.
Sublime uses a license model for its users, I'm gonna demonstrate how we bypass this.
Since I'm running W10 x64 we are going to be working with the x64 version, Olly do not support x86-64 binaries
so we need to grab some new tools, if you are familiar with IDA Pro you can use that, I will however
use the latest snapshot of [x64dbg](http://x64dbg.com/#start).

Our target can be found [here](https://www.sublimetext.com/3).

Before we start up our debugger let's do some checking around. I usually run a scanner
on the software I'm going to work on but in this case this won't be necessary since it's unprotected.

Let's fire up Sublime.

This is what we see.

![Sublime](/images/Sublime/S1.png)

Looking at the top we can clearly see the "(UNREGISTERED)" label. In the "Help" -> "About box" we can see something similar.
![Sublime](/images/Sublime/S2.png)
