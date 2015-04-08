---
layout: post
title: Swift "pitfalls"
published: true
categories:
date: 2015-04-08 08:24:08
---

I've been doing iOS and Mac OS programming in Swift for a while and I noticed some "pitfalls". They are some syntax details that may cause compiler errors and leave you wonder, "why is that". I believe that all the "pitfalls" here are included in the Swift official documentation and tutorials already. But I think it would help myself and other people if I put them all together here in one place.


<br />

## Naming of methods

The following code won't work:
{% highlight swift %}
class WeirdClass {
	
	class func println(string:String) {
		
		println("---\(string)---")
		// The above line will call itself i.e.
		// `WeirdClass.println(string:String)`
		// instead of system's println
		// leading to an infinite cycle
	}
	
	init() {
		WeirdClass.println("init")
	}
}
{% endhighlight %}

<br />

## Object does not respond to selector when its class is not a subclass of `NSObject`.

You can't do:
{% highlight swift %}
// Does not work
class EasyTimerManager { // Not a subclass of `NSObject`
	var timer:NSTimer?

	...

	func setUpTimer() {
		timer = NSTimer.scheduledTimerWithTimeInterval(
			self.timeInterval,
			target: self,
			selector: "timerFired:",
			userInfo: nil,
			repeats: false
		)
	}

	func timerFired(sender:AnyObject) {
		...
	}
}
{% endhighlight %}

Eror message:

{% highlight bash %}
*** NSForwarding: warning: object 0x7ff7630698c0 of class 'YourProject.EasyTimerManager' does not implement methodSignatureForSelector: -- trouble ahead
Unrecognized selector -[YourProject.EasyTimerManager timerFired:]
{% endhighlight %}

Fix:

{% highlight swift %}

// Works!
class EasyTimerManager : NSObject {
	...
	func timerFired(sender:AnyObject) {
		...
	}
}
{% endhighlight %}

<br />

## Object does not respond to selector if the function is private.

You can't do:
{% highlight swift %}
// Does not work either
class EasyTimerManager : NSObject {
	...

	func setUpTimer() {
		timer = NSTimer.scheduledTimerWithTimeInterval(
			self.timeInterval,
			target: self,
			selector: "timerFired:",
			userInfo: nil,
			repeats: false
		)
	}

	private func timerFired(sender:AnyObject) { // Private!
		...
	}
}
{% endhighlight %}

Error message:

{% highlight bash %}
-[YourProject.EasyTimerManager timerFired:]: unrecognized selector sent to instance 0x7a1cc610
*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '-[YourProject.EasyTimerManager timerFired:]: unrecognized selector sent to instance 0x7a1cc610'
{% endhighlight %}

Fix:

{% highlight swift %}

// Works!
class EasyTimerManager : NSObject {
	...
	func timerFired(sender:AnyObject) {
		...
	}
}

{% endhighlight %}

<br />

## Single line expression in closures: Implicit 'return'

{% highlight swift %}
func moveViewToCenter() {
	...
}

func moveViewToCenterReturnsTrueIfStillVisible() -> Bool {
	...
}

func performViewAnimation() {

	// Works!
	let animationClosure: { () -> Void in
		moveViewToCenter()
	})
					
	// Does not work.
	let invalidClosure: { () -> Void in
		moveViewToCenterReturnsTrueIfStillVisible()

		// This is equivalent to writing
		// `return moveViewToCenterReturnsTrueIfStillVisible()`
		// the function returns a `Bool` and
		// does not match the closure's return type `Void`
	})

	...
}

{% endhighlight %}

Fix:
{% highlight swift %}
func performViewAnimation() {
	// Works!
	let animationClosure: { () -> Void in
		moveViewToCenterReturnsTrueIfStillVisible()
		return
	})

	...
}
{% endhighlight %}

<br />

## Closures that takes no parameters and returns `Void`

{% highlight swift %}
// Valid!
let transformationClosure = { () -> () in
    self.awesomeView.transform = CGAffineTransformIdentity
}

// Valid!
let awesomeClosure = { () -> Void in
    self.awesomeBool = true
}

// Error! Invalid!
let invalidClosure = { Void -> () in
    //does not work
    ...
}
{% endhighlight %}

<br />

## Not a Swift pitfall - happened when writing Objective-C after writing a lot of Swift

This is probably is not a Swift pitfall - happened to me when writing Objective-C after writing a lot of Swift.

In Swift you can do something like this:

{% highlight swift %}
var error:NSError?
{% endhighlight %}

And error will be nil. You don't have to initialise `error` to `nil` any more

Then the problem occurred after returning to Objective-C. I wrote

{% highlight objc %}
NSError *error;

// Operations
...

if (error == nil) {
	// Success, NEVER!
} else {
	// Failure, ALWAYS!
}
{% endhighlight %}

Then it never succeeds! The error is never nil! Even though I get the ideal result from the operations all the time.

Then I realised I should write

{% highlight objc %}
NSError *error = nil;
{% endhighlight %}