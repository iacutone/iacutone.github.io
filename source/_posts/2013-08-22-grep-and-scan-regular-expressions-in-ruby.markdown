---
layout: post
title: "grep and scan methods with Regular Expressions in Ruby"
date: 2013-08-22 18:56
comments: true
categories: regex ruby grep
---

<p>I am creating an app to provide food cart locations in Manhattan.  <a href='https://github.com/iacutone/food-truck'>Here</a> is the code.  Food carts generally post their locations on Twitter; in order to create latitude and longitude coordinates, I needed to parse tweets.  This is where scan and grep play their role, to match elements in a tweet string.  Let's say this is our tweet: "53rd and park. Ready by 11!"  This string is database column in my locations model.</p>

<p>Code for scan method.</p>
{% codeblock lang:ruby %}

	tweet = "53rd and park. Ready by 11!"
	tweet.scan(/53/)
	# => ["53"] 
	tweet.scan(/Park/i)
	# => ["park"]

{% endcodeblock %}

<p>The scan method takes a string as an input.  Also, the i is used in the regular expression in order to make it case insensitive.</p>
{% codeblock lang:ruby %}

	tweet.scan(/Park/)
	#  => [] 

{% endcodeblock %}  

<p>Code for grep method.</p>
{% codeblock lang:ruby %}

	tweet = "53rd and park. Ready by 11!"
	tweet_array = tweet.split
	# => ["53rd", "and", "park.", "Ready", "by", "11!"] 
	tweet_array.grep(/53/)
	# => ["53rd"]
	tweet_array.grep(/Park/i)
	# => ["park."]

{% endcodeblock %}

<p>The grep method takes an array as an input, so use the string split method to turn it into an array seperated by commas.  Also, the regular expression matches the entire string.  The grep method seems to be more suited as an alternative for the select and map methods.  <a href='http://zigzag.github.io/2010/03/31/grep-in-ruby----a-powerful-enumerable-method.html'>This</a> blog post goes into further detail explaining more cases to use the grep method over more familiar methods.<p>
