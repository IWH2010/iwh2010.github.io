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
In the "References" search for "license", go to the first one @Adress "00007FF79C554860".
Put a BP here and rerun, do we break?

Indeed we do, now think with me, why do we break?
The reason is that the software has a routine that checks on running if we are registered or not, it does this by
checking a file called "License.sublime_license", this file resides in "AppData\Roaming\Sublime Text 3\Local" or in
"Data\Local" if you are using the portable version.

Lets step into the first call and do some browsing. After running around for a while we should end up here:

![Sublime](/images/Sublime/S4.png)


00007FF79C7AB5E0 | E8 F3 AC FF FF           | call sublime_text.7FF79C7A62D8          |
00007FF79C7AB5E5 | 89 86 C4 00 00 00        | mov dword ptr ds:[rsi+C4],eax           |
00007FF79C7AB5EB | 8D 55 06                 | lea edx,dword ptr ss:[rbp+6]            |
00007FF79C7AB5EE | 48 8B CF                 | mov rcx,rdi                             |
00007FF79C7AB5F1 | E8 E2 AC FF FF           | call sublime_text.7FF79C7A62D8          |
00007FF79C7AB5F6 | 89 86 C8 00 00 00        | mov dword ptr ds:[rsi+C8],eax           |
00007FF79C7AB5FC | 8D 55 08                 | lea edx,dword ptr ss:[rbp+8]            |
00007FF79C7AB5FF | 48 8B CF                 | mov rcx,rdi                             |
00007FF79C7AB602 | E8 D1 AC FF FF           | call sublime_text.7FF79C7A62D8          |
00007FF79C7AB607 | 83 F8 01                 | cmp eax,1                               | <<<<<<<<<<
00007FF79C7AB60A | 0F 94 C0                 | sete al                                 |
00007FF79C7AB60D | 88 86 08 01 00 00        | mov byte ptr ds:[rsi+108],al            |
00007FF79C7AB613 | 81 BE C8 00 00 00 00 F0  | cmp dword ptr ds:[rsi+C8],F000          |
00007FF79C7AB61D | 75 18                    | jne sublime_text.7FF79C7AB637           |
00007FF79C7AB61F | FF 15 AB FC 03 00        | call qword ptr ds:[<&GetCurrentThread>] |
00007FF79C7AB625 | 48 8B C8                 | mov rcx,rax                             |
00007FF79C7AB628 | FF 15 DA FE 03 00        | call qword ptr ds:[<&GetThreadPriority> |

Lets be honest, if i didn't play with the registration function in the "About" -> "License" box I never would had figured this out.
It works in the same way, it compares the value of EAX with 1, however, when the license is read and declared invalid, EAX
holds the value of 2. All we have to do now is simply change the line "cmp eax,1" to "comp eax,2", save the patch and run.

!(Sublime)[/images/Sublime/S5.png]

No "(UNREGISTERED)" label at the top.
Pressing the "About" tab asks us to remove the license.
