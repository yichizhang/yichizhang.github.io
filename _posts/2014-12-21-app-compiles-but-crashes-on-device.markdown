---
layout: post
title:  "Without linking framework, application compiles, runs on iOS 8 simulator and iOS 7 device, but crashes on iOS 8 device"
date:   2014-12-21 01:04:00
---

{% highlight console %}
Mac OS Version: OS X 10.10.1 (14B25)
XCode Version: Version 6.1.1 (6A2008a)
Simulator: Version 8.1 (550.3)
iPhone 6 Simulator OS Version: iOS 8.1 (12B411)
{% endhighlight %}

I was programming an iPhone application that requires accessing user's contacts.

I used [APAddressBook library](https://github.com/Alterplay/APAddressBook)

<br><br>

###The application complies; it works on my old iPhone 4 that runs iOS 7.1; works on simulator; but not on my iPhone 6 (iOS 8.1.2).

###The weird thing was, the application crashes right before the contacts view controller shows up; no error messages; setting breakpoints won't work; only prints this message:

{% highlight console %}
process launch failed: timed out trying to launch app
{% endhighlight %}

As I said, setting breakpoints won't work; the application just quits with no useful information provided, so I couldn't debug.

I tried:

1. Write the view controller in Objective-C, rather than Swift

2. Changed code signing from distribution to developer

Along with:

1. Constantly cleaning;

2. Constantly cleaning build folder

Wouldn't work.

<br><br>

##At the end, I realized that the problem was, I didn't link 'AddressBook.framework'.

So I went to target settings; added 'AddressBook.framework' to 'Linked Frameworks and Libraries'; cleaned project and cleaned build folder.

Ran the application on my device; the application still crashes, but it won't just disappear without any useful information. I got information about the error.

That's great. Although it still crashes, but at least I'm able to debug.