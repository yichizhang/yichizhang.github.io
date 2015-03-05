---
layout: post
title: Prescroll a UITableView
published: true
categories:
date: 2015-03-02 20:53:31
---

I was trying to "prescroll" a UITableView before showing it to the user. I initially tried doing it in "viewWillAppear:":

{% highlight swift %}
class PrescrollDemoViewController : UITableViewController {
	...
	var rowToPrescrollTo:Int?
	...

	override func viewWillAppear(animated: Bool) {
		super.viewWillAppear(animated)
		
		prescrollTableView(animated: false)
	}

	// MARK: Pre-scroll
	func prescrollTableView(#animated:Bool) {
		if let rowToPrescrollTo = rowToPrescrollTo {
			let indexPath = NSIndexPath(forRow: rowToPrescrollTo, inSection: 0)
			self.tableView.scrollToRowAtIndexPath(indexPath, atScrollPosition: .Top, animated: animated)
		}
	}
}
{% endhighlight %}

It works when I was trying to "prescroll" to Row 7, however, the result is not satisfactory when scrolling a bit more downward. I tried "prescrolling" to Row 18 (the last row), and it does not scroll to the correct position:

<img alt="prescroll in view will appear" src="{{ site.url }}/assets/2015/03/prescrollInViewWillAppear.png" width="370" />

Then I tried "prescrolling" in "viewDidAppear:":

{% highlight swift %}
class PrescrollDemoViewController : UITableViewController {
	...

	override func viewDidAppear(animated: Bool) {
		super.viewDidAppear(animated)
		
		prescrollTableView(animated: true)
	}

	...
}
{% endhighlight %}

Using this method, Row 18 scrolls to the correct position, but the user sees the whole "scrolling process":

![prescroll in view did appear]({{ site.url }}/assets/2015/03/prescrollInViewDidAppear.png)

How to "prescroll" to the correct indexPath you wish, without letting the user see the whole "scrolling process"? I figured out a way to do it, which is scrolling in "viewDidLayoutSubviews" with a Bool flag:

{% highlight swift %}
class PrescrollDemoViewController : UITableViewController {
	...
	var shouldPrescroll = false
	var rowToPrescrollTo:Int?
	...

	override func viewWillAppear(animated: Bool) {
		super.viewWillAppear(animated)
		
		shouldPrescroll = true
	}
	
	override func viewDidAppear(animated: Bool) {
		super.viewDidAppear(animated)
		
		shouldPrescroll = false
	}
	
	override func viewDidLayoutSubviews() {
		super.viewDidLayoutSubviews()
		
		if shouldPrescroll {
			prescrollTableView(animated: false)
		}
	}

	// MARK: Pre-scroll
	func prescrollTableView(#animated:Bool) {
		...
	}
}
{% endhighlight %}

Result:

![prescroll in view did layout subviews with a boolean flag]({{ site.url }}/assets/2015/03/prescrollInViewDidLayoutSubviews.png)


I put the source code of this method in [CrazyDemoViewController.swift](https://github.com/yichizhang/YZBasicCells/blob/master/BasicCells-Demo/BasicCells-Demo/CrazyDemoViewController.swift), as a part of the demo project of [YZBasicCells](https://github.com/yichizhang/YZBasicCells).