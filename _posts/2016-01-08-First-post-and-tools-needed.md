---
layout: post
title: First post and tools needed
---

Hello and welcome to my fresh blog. Here I will teach you the basics and the knowledge to bypass DRM.

DRM stands for Digital Rights Management and for us it simply means copy protection. [If you wanna learn more about it you can look it up at Wikipedia.]
(https://en.wikipedia.org/wiki/Digital_rights_management)

The DRM we are gonna work on are the ones developed for software and not games. 

Now before we get started we need to setup our workstation. This blog is made for Windows and the operating system I'm running is W10 with all it's monitoring glory. :smirk:

The debugger we are going to use is OllyDBG and IDA Pro, the version i'm using is 2.01 and can be found [here.](http://www.ollydbg.de/)
My version of IDA Pro is 6.8.150423 and can be found on the web. :smirk:

What we are gonna need next is software to detect if our target is protected. The most popular is PEiD however it has been disbanded, version 0.95 can be found [here.]
(https://tuts4you.com/download.php?view.398)
You will also need a database [userdb.txt - ](http://handlers.sans.org/jclausing/userdb.txt) this file shall recide in the same folder as PEiD.

I also recommend [Protection ID](http://pid.gamecopyworld.com/) - Your antivirus is gonna complain, whitelist it. [Stud PE](http://www.cgsoftlabs.ro/studpe.html) is also needed.

There are several forks of PEiD, such as [DiE](http://ntinfo.biz/) and [EXEinfo PE] (http://sourceforge.net/projects/exeinfope/). Both might come in handy later.

[Universal Extractor] (http://legroom.net/software/uniextract) is a must have, simply replace its outdated files with newer versions of 7-Zip and WinRAR and you are good to go.

These are the basic software needed, we will also need some plugins and PE tools. The plugins are mostly needed for Olly, i will take these up in a later post. 

[Portable Executable or just PE](https://en.wikipedia.org/wiki/Portable_Executable) is something i will go over since it's pretty big and will take alot of space.
We will use Import REConstructor, shortened impREC. [Can be found here] (https://tuts4you.com/download.php?view.415). 
We will also use [Imports Fixer](https://tuts4you.com/download.php?view.2969) and [Lord PE.] (http://www.softpedia.com/get/Programming/File-Editors/LordPE.shtml)

Now that we have our workstation up and running, lets get to the good parts. When reversing there are two things we should always do, these are static and dynamic analyze.
The difference being that static is done without actually running the program. How we actually do this is something i will show later. :innocent:

I will asume that my viewers have limited or no prior experience, hence i will try to explain as much as i can. I will start softly with basic stuff and pick up the pace as we learn more about this
fascinating subject. I hope that you as a reader will learn and experiment on your free time aswell. May the force be with you. :wink:





