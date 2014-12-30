---
layout: post
title:  "Python study notes for Objective-C programmers"
date:   2014-12-30 01:22:30
---

I don't write Python often and I always forget its syntax. Therefore, I'm keeping a note of the major differences between Python and Objective-C that I've learnt here. Hope it helps you too!

## Basics

Objective-C | Python
------------|-------
<code>&#124;&#124;</code> | <code>or</code>
<code>&&</code> | <code>and</code>
<code>YES</code> | <code>True</code>
<code>NO</code> | <code>False</code>

<br><br>

## String

Objective-C | Python
------------|-------
<code>string.<b>length</b></code> | <code><b>len</b>(string)</code>
<code>[string <b>hasPrefix</b>:prefix]</code> | <code>string.<b>startswith</b>(prefix)</code>
<code>[string <b>hasSuffix</b>:suffix]</code> | <code>string.<b>endswith</b>(suffix)</code>

<br><br>

## Comments

Objective-C
{% highlight objc %}
// comment
/*
 multiline
 comment
 */
{% endhighlight %}

Python
{% highlight python %}
# comment
'''
 multiline
 comment
'''
{% endhighlight %}

<br><br>

## str()

Objective-C
{% highlight objc %}

NSDictionary *dict = @{ @"Name": @"Zara", @"Age": @"7" };
NSLog("Description: %@", [dict description]);

{% endhighlight %}

Python
{% highlight python %}

dict = {'Name': 'Zara', 'Age': 7};
print "Equivalent String : %s" % str (dict) # str()

{% endhighlight %}

<br><br>

## Other

C / Objective-C
{% highlight objc %}

if (condition1){
	
}else if (condition2){
	statement2;
}else {
	statement3;
}

{% endhighlight %}

Python
{% highlight python %}

if condition1:		# note the colon
	pass			# use pass for an empty code block
elif condition2:	# elif = else if
	statement2
else:				# don't forget the ':'
	statement3

{% endhighlight %}