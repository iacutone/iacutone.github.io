---
layout: post
title: "Submitting a Get Request in Rails"
date: 2016-02-05 15:04:18 -0500
comments: true
categories: rails
---

For the error "WARNING: Can't verify CSRF token authenticity", add a CSRF token.

``` ruby _form.html.haml
= form_tag some_path(@organization), method: :get do
  = hidden_field_tag :authenticity_token, form_authenticity_token # this needs to be added!
  = submit_tag "View"
  = link_to_function 'Export', "Javascript function which posts to another route"
```

The top-level block contains method: :get which doesn't auto-add the needed authenticity_token.


### Helpful Links

[Understanding the Rails Authenticity Token](http://stackoverflow.com/questions/941594/understanding-the-rails-authenticity-token)

