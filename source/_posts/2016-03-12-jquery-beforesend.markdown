---
layout: post
title: JQuery beforeSend Function
date: 2016-03-12 15:32:09 -0500
comments: true
categories: javascript jquery ajax
---

I have a form which submits a comment via AJAX. The app was throwing validation errors if there was no value in the comment field. My initial solution was to disable the submit button if the comment is blank. However, using the `beforeSend` function provides a cleaner solution.

```coffeescript
  comment = ('.form-field-comment').val()

  $.ajax
    type: 'POST'
    dataType: 'script'
    url: comment_form.attr('action')
    data:
      comment:
        body: comment
    beforeSend: () ->
      if comment is ''
        return false; # do this instead of disabling the submit button 
```

With the above code, the comment form is never posted if the comment is blank.
