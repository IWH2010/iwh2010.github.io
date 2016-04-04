---
layout: post
title: Beginning with basic reversing
---

Lets start this lesson with configuring OllyDBG. Start by running Olly. When started click "Options". A small box should appear.

![olly_options](/images/olly_options.png)

Go to "Exceptions" and make sure it looks as this.

![olly_exceptions](/images/olly_exceptions.png)

Later in our studies we will come across a program that has the ability to detect if its being debugged.
If that happens it will pass an exception that will make Olly either hang, crash or get stuck in and endless loop. These changes bypass the most common exceptions.
We also enabled the ability to enter custom ranges which will come in handy later.

Next we go to the "Directories" tab. We first create two folders in our OllyDBG folder, these are called "Plugins" and "UDD".
UDD are where session files are saved and the Plugins are for .dll files adding new features.

Next we make sure to link to these folders in Olly.

![olly_directories](/images/olly_directories.png)

The last thing we do before clicking "OK" is changing the Scheme.
Head over to "Code-highlightning" and change the scheme from "Christmas Tree" to "Jumps and Calls".
Click "OK" to save all changes.

![olly_scheme](/images/olly_scheme.png)

Now we will need some plugins. We start with something that lets us dump our target, very useful when dealing with packers. [OllyDumpEx 1.40](https://tuts4you.com/download.php?view.3212).
Simply put *OllyDumpEx_Od20.dll* into your plugins folder. Next we add something to give us more functionality. [OD2-ExPlug 201.15](https://tuts4you.com/download.php?view.3482).
Your anti-virus might complain, ignore. Unpack the content from the *Bin* folder into your plugins folder, you can skip *OD2ExPlug.v2.0.14.03.TXT.*

The last plugin we will need is [OllyExt 1.8](https://tuts4you.com/download.php?view.3392). This nifty thing makes our debugger undetectable to the most common tricks used for anti-debugging.
Simply extract *OllyExt.dll* into your plugins folder.

We are now done with the configuration of OllyDBG. To check if the plugins were correctly installed, simply click the "plugins" tab and check for yourself. :wink:
