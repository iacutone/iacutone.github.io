---
layout: post
title: "Auto Layout Xcode for Navigation Bar"
date: 2015-11-13 19:03:02 -0500
comments: true
categories: swift xcode
---

Finding auto layout information is difficult. The steps below pin a navigation bar to the top of the screen for all iOS devices.

{% img center /images/constraints.gif  %}

### Steps
* Make sure "Navigation Bar" is selected on the left menu
* Click the "Pin" button on the bottom right corner
* Click the top, left and right constraints
* Click the Height constraint
* Add the 4 contraints 

The above steps "pin" the navigation bar object to the top of the view and stretch the navigation bar to the sides of the view. The height constraint keeps a static height for the navigation bar.


Helpful Links

[Auto Layout Basics](https://github.com/codepath/ios_guides/wiki/Auto-Layout-Basics)
