---
layout: post
title: "Prototypes and Constructors in Javascript"
date: 2015-11-07 12:57:08 -0500
comments: true
categories: javascript
---

I usually do not need to write code using neither constructors nor prototypes in Javascript. So, I always forget how to write them when I need to use them.

## Constructor

``` javascript
  Say = function(){
    this.hello = function() {
      return "Hello";
    }
  }

  var say = new Say();
  say.hello()

  // "Hello"

```

## Prototype

``` javascript
  function Say() {}
   Say.prototype.hello = function() {
    return "Hello";
  }

  var say = new Say();
  say.hello()

  // "Hello"
```
 

Helpful Links

[Use of 'prototype' vs. 'this' in JavaScript?](http://stackoverflow.com/questions/310870/use-of-prototype-vs-this-in-javascript)

[Advantages of using prototype, vs defining methods straight in the constructor?](http://stackoverflow.com/questions/4508313/advantages-of-using-prototype-vs-defining-methods-straight-in-the-constructor)
