---
layout: post
title: iOS 8 & iOS 9 Custom Keyboard - Custom Height
published: true
categories:
date: 2015-10-03 03:51:00
---

I've been building an iOS keyboard app: [AGK Ancient Greek Keyboard (Polytonic Greek)](https://itunes.apple.com/app/id1018791342)

In the app, I would like to have custom height for the keyboard, and the height of it changes depending on the orientation of the device (or depending the size of the screen, since from iOS 8, `[UIScreen bounds]` is interface-oriented).

### If you would like to view the full source code, please clone the repository for this blog post: 
[https://github.com/yichizhang/CustomHeightKeyboard.git](https://github.com/yichizhang/CustomHeightKeyboard)

### Solution: iOS 8 only (branch 'dev/a')

I found a solution in Stackoverflow. It's [skyline75489](http://stackoverflow.com/users/3562486/skyline75489)'s answer in [iOS 8 Custom Keyboard: Changing the Height](http://stackoverflow.com/questions/24167909/ios-8-custom-keyboard-changing-the-height).

It's basically adding an extra height constraint to `inputView` or `view` (NB: they are identical) in `updateViewConstraints:`. 

It works in iOS 8. The code:

{% highlight swift %}
class KeyboardViewController: UIInputViewController {
    
    /*
     * Please note: iOS 8 only. This solution does not work in iOS 9.
     */
    @IBOutlet var nextKeyboardButton: UIButton!
    var heightConstraint:NSLayoutConstraint!
    
    override func updateViewConstraints() {
        super.updateViewConstraints()
    
        // Add custom view sizing constraints here
        if (view.frame.size.width == 0 || 
            view.frame.size.height == 0) {
            return
        }
        
        setUpHeightConstraint()
    }

    override func viewDidLoad() {
        super.viewDidLoad()

        // Set up nextKeyboardButton
        // ...

        self.view.addSubview(self.nextKeyboardButton)

        // Set up constraints in the view    
        let nextKeyboardButtonLeftSideConstraint = NSLayoutConstraint(...)
        let nextKeyboardButtonBottomConstraint = NSLayoutConstraint(...)
        self.view.addConstraints([
            nextKeyboardButtonLeftSideConstraint, 
            nextKeyboardButtonBottomConstraint])
    }

    // ...

    func setUpHeightConstraint() {
        let customHeight = UIScreen.mainScreen().bounds.height / 2
        
        if heightConstraint == nil {
            heightConstraint = NSLayoutConstraint(
                item: self.view,
                attribute: .Height,
                relatedBy: .Equal,
                toItem: nil,
                attribute: .NotAnAttribute,
                multiplier: 1,
                constant: customHeight
            )
            heightConstraint.priority = UILayoutPriority(999)
            
            view.addConstraint(heightConstraint)
        } else {
            heightConstraint.constant = customHeight
        }
    }
}
{% endhighlight %}

To view the full source code, please clone the project, go to the project directory and run `checkout dev/a`.

You can also view the diff between this solution the code provided by Xcode keyboard extension template: <https://github.com/yichizhang/CustomHeightKeyboard/compare/dev/a?expand=1>

### Above solution does not fully work in iOS 9

### It does not change the keyboard height when it first appears

In iOS 9, the above solution does not change the keyboard height when it first appears. It only works when you rotate the screen. Please see image:

![iOS 9 solution A problem]({{ site.url }}/assets/2015/10/custom_keyboard_height_solution_a_problem.jpg)

### Workaround: supports iOS 8 and iOS 9 (branch 'dev/b')

I figured out a workaround for iOS 9. It's simple - just move constraints set up code to `viewDidAppear:`.

{% highlight swift %}
class KeyboardViewController: UIInputViewController {
    
    /*
     * Please note: Supports iOS 8 and iOS 9.
     */
    // ...

    override func viewDidLoad() {
        super.viewDidLoad()

        // Set up nextKeyboardButton
        // ...

        self.view.addSubview(self.nextKeyboardButton)

        // Please note: constraints in the view is now set up in viewDidAppear:
    }


    override func viewDidAppear(animated: Bool) {
        super.viewDidAppear(animated)

        // Set up constraints in the view    
        let nextKeyboardButtonLeftSideConstraint = NSLayoutConstraint(...)
        let nextKeyboardButtonBottomConstraint = NSLayoutConstraint(...)
        self.view.addConstraints([
            nextKeyboardButtonLeftSideConstraint, 
            nextKeyboardButtonBottomConstraint])
    }

    // ...

    func setUpHeightConstraint() {
        // ...
    }
}
{% endhighlight %}

To view the full source code, please clone the project, go to the project directory and run `checkout dev/b`.

You can also view the diff between this solution and the code provided by Xcode keyboard extension template: <https://github.com/yichizhang/CustomHeightKeyboard/compare/dev/b?expand=1>

### Conclusion

I'm not entirely sure how it works. I've tried putting methods like `setNeedsUpdateConstraints` and `layoutIfNeeded` in different places and it didn't work.

In Apple's [App Extension Programming Guide: Custom Keyboard](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/Keyboard.html), there is a note on keyboard height:

> Note
>
> In iOS 8.0, you can adjust a custom keyboard’s height any time after its primary view initially draws on screen.

This might gives us an idea about why putting constraints set up code in `viewDidAppear:` makes it work.

If you know why, please do not hesitate to leave a comment.

