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

Looking at the top we can clearly see the "(UNREGISTERED)" label. In the "Help" -> "About" box we can see something similar.
![Sublime](/images/Sublime/S2.png)

Let's open the binary in our debugger.
![Sublime](/images/Sublime/S3.png)

Right click anywhere, go "Search for" -> "Current module" -> "String references".
In the "References" window, search for "license", go to the first one @adress "00007FF79C554860".
Put a BP here and rerun, do we break?

Indeed we do, now think with me, why do we break?

The reason is that the software has a routine that checks on running if we are registered or not, it does this by
checking a file called "License.sublime_license", this file resides in "AppData\Roaming\Sublime Text 3\Local" or in
"Data\Local" if you are using the portable version.

Lets step into the first call and do some browsing. After running around for a while we should end up here:

![Sublime](/images/Sublime/S4.png)

00007FF79C7AB607: cmp eax,1
00007FF79C7AB60A: sete al

Lets be honest, if I didn't play with the registration function in the "About" -> "License" box I never would had figured this out.
It works in the same way, it compares the value of EAX with 1, however, when the license is read and declared invalid, EAX
holds the value of 2. All we have to do now is simply change the line "cmp eax,1" to "comp eax,2", save the patch and run.

![Sublime][/images/Sublime/S5.png]

No "(UNREGISTERED)" label at the top.
Pressing the "About" tab asks us to remove the license.

Congratulations, you are now running a legitimate text editor.
