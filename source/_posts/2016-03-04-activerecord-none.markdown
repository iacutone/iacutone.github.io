---
layout: post
title: "ActiveRecord none"
date: 2016-03-04 19:15:38 -0500
comments: true
categories: rails ruby
---

The .none class method was introduced in Rails 4.0.2 and is helpful when returning a blank array will break.

```ruby
Organization.none.paginate(page: '1')
 => #<ActiveRecord::Relation []>

[].paginate(page: '1')
 => NoMethodError: undefined method `paginate' for []:Array
```

### Helpful Links
[none](http://apidock.com/rails/v4.0.2/ActiveRecord/QueryMethods/none)
