---
layout: post
title: "ENV Variables in XCode"
date: 2015-12-03 18:02:14 -0500
comments: true
categories: xcode swift
---

Setting ENV variables in XCode in not straighforward. I have broken down how to accomplish setting up an ENV variable in the following videos and code example.

{% youtube r5HZwbkA52I %}
Make a new conifuration file and set any ENV variables you need.


{% youtube jwuQgWb58Xs %}
In the debug environment, use the debug.xcconfig file. Then, assign the value to something, in this case, SignUpUrl corresponds with the SIGN_UP_URL, which is set to localhost:4567/sign_up.


``` swift SignUpController.swift
  var info:[NSObject:AnyObject] = NSBundle.mainBundle().infoDictionary!
  var sign_up_url = info["SignUpUrl"] as! String
```

The sign_up_url variable is set to localhost:4567/signup.


Helpful Links

[Configuring ENV Variables in XCode](http://bogardon.github.io/xcode/environment-variables/2013/05/20/configuring-env-variables-in-xcode.html)
