---
layout: post
title: "Testing Your Way to Success"
date: 2013-10-22 21:36
comments: true
categories: rails testing rspec
---

<p>I have been immersed in production codebases for my <a href='fohrcard.com'>Fohr Card</a> and <a href='http://www.fracturedatlas.org/'>Fractured Atlas</a> projects.  At first, it feel initimidating trying to figure out all of the models and related associations.  I have found that a great way to wrap your head around the code base is to create a new branch and test the code.</p>

<p>A good basic set-up is to start unit testing the models.  In order to do this depends on if your codebase is using a relational database or something like Mongoid.</p>

<h3>Relational Databases</h3>
{% codeblock Gemfile lang:ruby %}

	group :development do
	  gem 'guard-rspec', require: false
	end

	group :test do
	   gem 'rspec-rails'
	   gem 'factory_girl_rails'
	   gem 'faker'
	   gem 'shoulda-matchers'
	   gem 'terminal-notifier-guard' 
	end

{% endcodeblock %}

<ul>
	<li>Guard RSpec waits for you to save your files and automatically runs your tests, thereby reducing the time needed to run tests.</li>
	<li>I like the RSpec syntax over Unit Test and Mini Test.</li>
	<li>Factory Girl Rails creates objects for your tests.  This gem deserves its own blog post. </li>
	<li>The Faker Gem is used in conjunction with Factory Girl Rails to create fake data.</li>
	<li>I like to use Shoulda Matchers to test validaitons and associations in the code.  This gem and firing up Rails Console to play with the objects in my testing is the crux to learning a new codebase.</li>
	<li>Some people find Terminal Notifier annoying, but it helps me know if my tests pass when I have Sublime maximized on my 11 inch MacBook Air screen.</li>
</ul>  

<h3>Mongoid</h3>
{% codeblock Gemfile lang:ruby %}

	group :development do
	  gem 'guard-rspec', require: false
	end

	group :test do
	  gem 'fabrication'
	  gem 'faker'
	  gem 'rspec-rails'
	  gem 'mongoid-rspec'
	  gem 'terminal-notifier-guard' 
	end

{% endcodeblock %}

<p>The gems for Mongoid testing are similar to those used in testing with a relational database.<p>
<ul>
	<li>The Fabrication gem is similar to building objects with Factory Girl, except the documentation website <a href='http://www.fabricationgem.org/'>rocks.</a></li>
	<li>The Mongoid RSpec gem is similar to Shoulda-Matchers</li>
</ul>

<h3>Gems</h3>
<a href='https://github.com/guard/guard-rspec'>Guard RSpec</a></br>
<a href='https://github.com/rspec/rspec-rails'>RSpec Rails</a></br>
<a href='https://github.com/thoughtbot/factory_girl_rails'>Factory Girl Rails</a></br>
<a href='https://github.com/btelles/faker'>Faker</a></br>
<a href='https://github.com/thoughtbot/shoulda-matchers'>Shoulda Matchers</a></br>
<a href='https://github.com/Springest/terminal-notifier-guard'>Terminal Notifier Guard</a></br>
<a href='https://github.com/paulelliott/fabrication'>Fabrication</a></br>
<a href='https://github.com/evansagge/mongoid-rspec'>Mongoid RSpec</a></br></br>

<h3>Other Resources</h3>
<p>I found this book invaluable in learning best practices in testing: <a href='https://leanpub.com/everydayrailsrspec'>Everday RSpec</a></br>h/t Victoria Friedman</p>
