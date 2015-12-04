---
layout: post
title: Ruby's Fetch Method
date: 2015-09-18 18:15:29 -0500
comments: true
categories: ruby rails
---

Checking for values in a Rails params hash is complicated. In this post we will use the #fetch method to ensure nil is not called on a params hash.

``` ruby searches_helper.rb
  if params[:search][:organization_id].present?
    # do things
  end
```

The code above will call #present? on nil if params[:search][:organization_id] does not exist.

A better way to write the conditional:

``` ruby searches_helper.rb
  if params[:search].present and params[:search][:organization_id].present?
    # do things
  end
```

Writing a similar conditional with #fetch:

``` ruby searches_helper.rb
  if params.fetch(:search, {}).fetch(:organization_id, nil).present?
    # do things
  end
```

Let's break this method down into simpler components:

``` ruby
  params = {}
  puts params.fetch(:search, {})
  // {}
  
  params[:search] = "searched!"
  puts params.fetch(:search, {})
  // "searched!"
  
  puts fetch(:organization_id, nil)
  // nil
  
  params[:organization_id] = 1
  puts params.fetch(:organization_id, nil)
  // 1
```

I like the ability to call #fetch with a default return value if one does not exist. This leads to better ways to handle nil or catch errors.
