---
layout: post
title: iOS 8 & iOS 9 Custom Keyboard - Custom Height
published: true
categories:
date: 2015-10-03 03:51:00
---

Full source code for the project: [https://github.com/yichizhang/CustomHeightKeyboard.git](https://github.com/yichizhang/CustomHeightKeyboard)

<br /><br />

I've been building an iOS keyboard app: [AGK Ancient Greek Keyboard (Polytonic Greek)](https://itunes.apple.com/app/id1018791342)

In the app, I would like to have custom height for the keyboard, and the height of it changes depending on the orientation of the device (or depending the size of the screen, since from iOS 8, `[UIScreen bounds]` is interface-oriented).

It should be possible because in Apple's [App Extension Programming Guide: Custom Keyboard](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/Keyboard.html), there is a note on keyboard height:

> Note
>
> In iOS 8.0, you can adjust a custom keyboard’s height any time after its primary view initially draws on screen.

### The solution (works for both iOS 8 and iOS 9)
I found a solution in Stackoverflow. It's [skyline75489](http://stackoverflow.com/users/3562486/skyline75489)'s answer in [iOS 8 Custom Keyboard: Changing the Height](http://stackoverflow.com/questions/24167909/ios-8-custom-keyboard-changing-the-height).

It's basically adding an extra height constraint to `inputView` or `view` (NB: they are identical) in `updateViewConstraints:`. 

It works perfectly in iOS 8, but not iOS 9. I figured out a workaround for iOS 9. It’s simple - just move 'next keyboard button' constraints set up code to `viewDidAppear:`.

<script src="https://gist.github.com/yichizhang/67f4cded839dd2ff1d4183dd67c5edbc.js"></script>

Full source code: [https://github.com/yichizhang/CustomHeightKeyboard.git](https://github.com/yichizhang/CustomHeightKeyboard)

### Conclusion

I’m not entirely sure how it works. I’ve tried putting methods like `setNeedsUpdateConstraints` and `layoutIfNeeded` in different places and it didn’t work.

If you know why, please do not hesitate to leave a comment.
