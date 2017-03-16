---
layout: post
title: Swift "pitfalls"
published: true
categories:
date: 2015-04-08 08:24:08
---

*2017-03-16: Updated for Swift 3.0*

I've been doing iOS and Mac OS programming in Swift for a while and I noticed some "pitfalls". They are some syntax details that may cause compiler errors and leave you wonder, "why is that". I believe that all the "pitfalls" here are included in the Swift official documentation and tutorials already. But I think it would help myself and other people if I put them all together here in one place.


<br />

## 1. Naming of methods

The following code won't work:

{% highlight swift %}
class TestClass {
  class func print(_ string: String) {
    print("Test: \(string)")
    // The above line will call itself i.e. `TestClass.print(_:)`
    // instead of `print(_:​separator:​terminator:​)` in Swift Standard Library
    // leading to an infinite cycle
  }
  
  init() {
    TestClass.print("init")
  }
}
{% endhighlight %}

Fix:
{% highlight swift %}
  class func print(_ string: String) {
    Swift.print("Test: \(string)")
  }
{% endhighlight %}

<br />

## 2. Exposing method to Objective-C

In Swift 3.0, the following code won't compile:

{% highlight swift %}
// Does not work
class TestClass { // Not a subclass of `NSObject`
  func setUp() {
    button.addTarget(
      self,
      action: #selector(buttonTapped(_:)),
      for: .touchUpInside)
  }

  private func buttonTapped(sender: Any) { // Private
  }
}
{% endhighlight %}

Error message would be `Argument of '#selector' refers to instance method 'buttonTapped' that is not exposed to Objective-C'`

<br />

Fix by adding `@objc` to the method:

{% highlight swift %}
class TestClass {
  @objc private func buttonTapped(sender: Any) {
  }
}
{% endhighlight %}

Thanks to reader Jon Showing for their input.

<br />

## 3. Single line expression in closures: Implicit 'return'

{% highlight swift %}
func moveViewsToCenter() {
}

func makeViewsVisible() -> Bool {
}

func performAnimation() {
  // Works!
  let animationsA = { () -> Void in
    self.moveViewsToCenter()
  }
          
  // Swift 2.3: Won't compile
  // Swift 3.0: Warning with message
  //   "Result of call to 'makeViewsVisible()' is unused"
  let animationsB = { () -> Void in
    self.makeViewsVisible() 
  }

  // Works!
  let animationsC = { () -> Bool in
    self.makeViewsVisible()
    // This is equivalent to writing
    // `return self.makeViewsVisible()`
  }
}
{% endhighlight %}

<br />

Fix by either adding `@discardableResult` to the method,

{% highlight swift %}
@discardableResult func makeViewsVisible() -> Bool {
}
{% endhighlight %}

or 

{% highlight swift %}
  let animationsB = { () -> Void in
    let _ = self.makeViewsVisible()
  }
{% endhighlight %}

<br />

## 4. Closures that take no parameters and return `Void`

You COULD write: `() -> ()` OR `() -> Void`

{% highlight swift %}
// Valid!
let closure = { () -> () in
}

// Valid!
let closure = { () -> Void in
}
{% endhighlight %}

BUT NOT <strike>Void -> ()</strike>

{% highlight swift %}
// ERROR! Invalid!
let invalidClosure = { /* ERROR! */ Void -> () in
}
{% endhighlight %}

<br />

But, the easiest is probably:

{% highlight swift %}
// Valid!
let closure = {
  // Your code
}
{% endhighlight %}

<br />
