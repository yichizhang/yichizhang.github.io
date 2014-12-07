---
layout: post
title:  "Switch on auto complete in Terminal"
date:   2014-12-07 20:49:22
---

Edit the .inputrc file in your Home directory. The file might not exist.

{% highlight console %}
vi .inputrc
{% endhighlight %}

Add the following content:

{% highlight console %}
set completion-ignore-case on
set show-all-if-ambiguous on
TAB: menu-complete
{% endhighlight %}

Relaunch Terminal. Auto complete should be switched on now. Cycle through all the auto complete options by pressing TAB.