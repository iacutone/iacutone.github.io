---
layout: post
title: "Hiding Multiple Checkboxes with JQuery"
date: 2013-08-15 21:09
comments: true
categories: rails jquery coffeescript
---

<p>I have been developing a <a href='https://github.com/iacutone/new-quiz'>quiz app</a> for practice with a more complex schema interface.  I wanted to dive deeper into CoffeScript and found the opportunity while trying to figure out how a student can add an answer to a given quiz.  If a checkbox for a given answer is checked, I wanted the remaining checkboxes to disappear.  Also, if the student checked the incorrect box, the browser should render all checkboxes again.  Here is my <a href='https://gist.github.com/iacutone/c28e8fa1324f82b508e0'>code.</a></p>

{% codeblock questions.js.coffee lang:js %}

	$('form').on 'click', '.checker', (event) ->
		boxes = $(":checkbox:checked")
		nboxes = $(":checkbox:not(:checked)")
		if boxes.length == 1
			$('.checker_label').hide()
			nboxes.hide()
		if boxes.length == 0
			$('.checker_label').show()
			nboxes.show()

{% endcodeblock %}

<p>... and the view</p>

{% codeblock _form.html.haml lang:ruby %}

Quiz:
= @quiz.name

= form_for @question do |f|
	= f.hidden_field :quiz_id, value: @quiz.id
	= f.label "What is your question?"
	= f.text_field :content
	%br
	%fieldset
		= f.fields_for :answers do |builder|
			= builder.label :content, "Answer"
			= builder.text_field :content
			= builder.label "Correct answer?", class:'checker_label'
			= builder.check_box :is_correct, class: 'checker'
		%br
	= link_to_add_fields "Add an answer choice", f, :answers
	%br
	= f.submit

{% endcodeblock %}



