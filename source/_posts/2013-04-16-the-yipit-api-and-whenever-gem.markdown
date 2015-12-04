---
layout: post
title: "The Yipit API and Whenever Gem"
date: 2013-04-16 17:05
comments: true
categories: 
---

<p>Last week Alex and I presented Platt101 at our NYC on Rails Meetup, at the Flatiron School.  Platt101 let's a user write reviews about restaurants they have been to and renders those restaurants on a Google Map.  The restaurants are based on the 101 best according to New York Magazine.  The restaurants a user has been to render in a different color than restaurants a user hasn't had the pleasure of dining.  A fellow student (h/t Anthony) suggested we impliment a GroupOn link in our restaurant list in order for a user to provide the user incentive to go dine at given restaurants.</p>

<p> We decided to use the <a href='http://yipit.com/about/api/'>Yipit API</a> in order to aggregate all discount incentives from numerous discount sites such as GroupOn.  We accessed the Yipit API via the Yipit <a href='https://github.com/gangster/yipit'>gem</a> for Ruby on Rails, created by Gangster.  Aside from our client session running a few times, the gem provided for all of our data parsing needs.  The Yelp API we used to seed our database with relevant restaurant information included phone numbers.  We searched the phone numbers in our database against phone numbers on Yipit in order to find deals for the given restaurants on our list.  This functionality is provided with a rake task.  Here is the code: </p><br>

<p><img src = "/images/yipit.png" height = "500" width = "500"></p>

<p> The rake task provides us the ability to use the Whenever gem in order to run the rake task everyday.  This way our database in reseeded with new data about restaurant deals on a daily basis.  The Whenever gem was easy to impliment.</p><br>

<p><img src = "/images/whenever-2.png" height = "500" width = "500"></p>

<p>This code is automatically added to your config > schedule.rb when you run whenever . in terminal.  You can customize Whenever to run whenever you want.</p>

<p>Now, this all works well in development, but Platt101 is deployed on Heroku.  In production, we needed to reset up the Whenever gem.  This is also easy to accomplish on Heroku. Use <a href='https://addons.heroku.com/scheduler'>Heroku Scheduler</a> in order to run a chron job in production.  They do make you use input your credit card info, which is whack, but it says free... we shall see.</p>

<a href='http://www.platt101.com'>Platt101 can be found here.</a>

