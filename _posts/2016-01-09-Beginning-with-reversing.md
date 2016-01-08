---
layout: post
title: Beginning with basic reversing
---

Lets start this lesson with configuring OllyDBG. Start by running Olly. When started click "Options". A small box should appear.

![]({{ site.baseurl }}/images/olly_options.png "")

Go to "Exceptions" and make sure it looks as this.

![]({{ site.baseurl }}/images/olly_exceptions.png "")

Next we go to the "Directories" tab. We first create two folders in our OllyDBG folder, these are called "Plugins" and "UDD".
UDD are where session files are saved and the Plugins are for .dll files adding new features.

Next we make sure to link to these folders in Olly.

![]({{ site.baseurl }}/images/olly_directories.png "")

The last thing we do before clicking "OK" is changing the Scheme.
Head over to "Code-highlightning" and change the scheme from "Christmas Tree" to "Jumps and Calls".
Click "OK" to save all changes.

![]({{ site.baseurl }}/images/olly_scheme.png "")
