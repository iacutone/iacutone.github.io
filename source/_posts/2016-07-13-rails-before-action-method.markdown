---
layout: post
title: Rails before_action method
date: 2016-07-13 13:31:20 -0400
comments: true
categories: ruby rails
---

I did not know you could give a `before_action` a block argument. This is a benefit because you do not have to create a ruby method and pass the method into the `before_action`.

This got me thinking, what does `before_action` look like in Rails? The following is the source code from Rails 4.2.7, from `AbstractController::Callbacks::ClassMethods`.

```ruby
# :method: before_action
#
# :call-seq: before_action(names, block)
#
# Append a callback before actions. See _insert_callbacks for parameter details.

# Take callback names and an optional callback proc, normalize them,
# then call the block with each callback. This allows us to abstract
# the normalization across several methods that use it.
#
# ==== Parameters
# * <tt>callbacks</tt> - An array of callbacks, with an optional
#   options hash as the last parameter.
# * <tt>block</tt>    - A proc that should be added to the callbacks.
#
# ==== Block Parameters
# * <tt>name</tt>     - The callback to be added
# * <tt>options</tt>  - A hash of options to be used when adding the callback

def _insert_callbacks(callbacks, block = nil)
  options = callbacks.extract_options!
  _normalize_callback_options(options)
  callbacks.push(block) if block
  callbacks.each do |callback|
    yield callback, options
  end
end

[:before, :after, :around].each do |callback|
  define_method "#{callback}_action" do |*names, &blk|
    _insert_callbacks(names, blk) do |name, options|
      set_callback(:process_action, callback, name, options)
    end
  end
end
```

And I can do cool things like:

```ruby
before_action -> {
  @foo = Model.find params[:bar]
}
```

I would like to point out the `set_callback` method.

```ruby
before_action :authenticate

# can also be called like this
set_callback :process_action, :before, :authenticate
```
