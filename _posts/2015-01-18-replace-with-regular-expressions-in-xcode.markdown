---
layout: post
title: Replace with Regular Expressions in Xcode
published: true
categories:
date: 2015-01-18 12:49:02
---

##Find all occurrences of <code>icon_name_of_icon</code> and replace them with <code>ion_name_of_icon</code>

In the 2.x version of [sweetmandm/ionicons-iOS](https://github.com/sweetmandm/ionicons-iOS), the format of icon code constants have changed from <code>icon_name_of_icon</code> to <code>ion_name_of_icon</code>; <code>icon_ios7_name_of_icon</code> to <code>ion_ios_name_of_icon</code>

For example:

Version 1.x
{% highlight objc %}

icon_camera
icon_navicon
icon_ios7_email

{% endhighlight %}

Version 2.x
{% highlight objc %}

ion_camera
ion_navicon
ion_ios_email

{% endhighlight %}

###Solution

Search:

{% highlight bash %}

icon_ios7_([_a-zA-Z]*)

{% endhighlight %}

Replace:

{% highlight bash %}

ion_ios_$1

{% endhighlight %}

<br><br>

Search:

{% highlight bash %}

icon_([_a-zA-Z]*)

{% endhighlight %}

Replace:

{% highlight bash %}

ion_$1

{% endhighlight %}