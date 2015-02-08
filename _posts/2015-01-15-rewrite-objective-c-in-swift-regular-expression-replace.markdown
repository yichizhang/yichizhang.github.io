---
layout: post
title: Rewrite Objective-C code in Swift (Replace with Regular Expressions)
published: true
categories:
date: 2015-01-15 00:28:19
---

###Rewrite Objective-C code in Swift with the help of XCode's Regular Expression replace:

##Property declarations
{% highlight objc %}

@property (nonatomic, strong) NSArray *array1;
@property (nonatomic, strong) NSArray *array2;
@property (nonatomic, strong) NSArray *array3;
@property (nonatomic, strong) NSMutableDictionary *dictionary;
@property (nonatomic, strong) AwesomeObject *object;
@property (nonatomic, assign) BOOL awesomeValue;

{% endhighlight %}

Replace:

<code>@property \((.*)\) (.*) \*?(.*);</code>

With:

<code>var $3:$2?</code>

Result:
{% highlight objc %}

var array1:NSArray?
var array2:NSArray?
var array3:NSArray?
var dictionary:NSMutableDictionary?
var object:AwesomeObject?
var awesomeValue:BOOL?

{% endhighlight %}
