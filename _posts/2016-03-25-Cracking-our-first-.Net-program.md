---
layout: post
title: Cracking our first .Net program
---
[WIP]

Hello readers, I know that it's been a while but here we go, our first program is going to get bypassed.
Lets start with some basic information about .NET. .Net is developed by Microsoft and is similar to java, it uses its own VM (Virtual Machine)
to execute code, making it runnable on a non Microsoft OS. Microsoft Intermediate Language or MSIL or CIL (Common Intermediate Language) is an object-oriented assembly language.

Our target is Xpacker 2016 Beta 1, a modding tool used to mod the WWE games. A quick scan with PEiD gives "Microsoft Visual C# / Basic .NET", nothing about it being packed (it is),
however Protection ID gives us, "dotFuscator detected!".

Now we need some new tools, Olly does not work that well with .NET and there is a good reason for it. Since .NET has its own VM and is a bytecode language, reversing is much simpler.
It has the same problems that Java has, its super simple to decompile.

We will use [.NET Reflector](https://www.red-gate.com/products/dotnet-development/reflector/) along with the plugin [Reflexil](http://reflexil.net/).

Reflector along with reflexil allows us to see the inner workings of our software,  
