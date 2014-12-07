---
layout: post
title:  "Switch on auto complete in Terminal"
date:   2014-12-07 20:49:22
---

How to enable TAB auto completion in Mac OS X Terminal? It's easy.

Firstly, edit the .inputrc file in your Home directory. Execute the following commands:

{% highlight console %}
cd ~
vi .inputrc
{% endhighlight %}

Then, add the following content:

{% highlight console %}
set completion-ignore-case on
set show-all-if-ambiguous on
TAB: menu-complete
{% endhighlight %}

Relaunch Terminal. Auto complete should be switched on now. Cycle through all the auto complete options by pressing TAB.