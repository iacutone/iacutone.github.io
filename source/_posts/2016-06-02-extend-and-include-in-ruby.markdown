---
layout: post
title: Extend and Include in Ruby
date: 2016-06-02 15:25:45 -0500
comments: true
categories: ruby rails
---

I have been trying to clean up some old code with Ruby modules. This post is to help me remember the differences between `include` and `extend` in Ruby.

```ruby
class Foo
  extend ActionView::Helpers::NumberHelper
end
```

```ruby
module Foo
  extend ActionView::Helpers::NumberHelper
end
```

```ruby
Foo.number_to_currency 2
 => "$2.00"
```

```ruby
class Foo
  include ActionView::Helpers::NumberHelper
end
```

```ruby
foo = Foo.new
foo.number_to_currency 2
 => "$2.00"
```
