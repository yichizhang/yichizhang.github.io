---
layout: post
title:  "An interesting thing about '!=' in Swift"
date:   2014-12-27 04:26:00
---

I've been programming in Swift today.

Coming from a C/Objective-C background, I assumed the following code would work:

{% highlight swift %}
// won't compile
if (someVar!=nil) {

   ...
}
{% endhighlight %}

It won't work; the compiler must sees it as something like <code>someVar! = nil</code> due to optional chaining.

Instead, you have to write

{% highlight swift %}
if (someVar != nil) {

   ...
}
{% endhighlight %}

