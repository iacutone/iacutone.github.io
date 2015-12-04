---
layout: post
title: "Pluck Method in Active Record"
date: 2013-06-23 01:59
comments: true
categories: Rails 
---

<p>The past few days, I have been building a database to store some information for my friend Shari, a Peace Corps recruiter.  The database includes, a name, email and phone number.  Things got tricky building a date/time models and validations.  Every given time has six time slots for a user to sign up.  My conclusion to this problem was to create a different database column for every given time slot.  Then, I used the <a href='http://apidock.com/rails/ActiveRecord/Calculations/pluck'>pluck method</a> on my Active Record database in order to turn all of my time columns into strings in an array.  However, in Rails 3, pluck only excepts one column to turn into an array.  This feature is being changed in Rails 4 according to <a href ='http://meltingice.net/2013/06/11/pluck-multiple-columns-rails/'>this</a> blog post where he describes how to tamper with the the Active Record library or create a module to provide a pluck_all method for a Rails 3 application.  I implemented this code, and was returned an array of hashes which did not help my cause.  But this is an interesting concept and it was fun and insightful reading the source code.</p>

{% codeblock users_controller.rb lang:rb %}

  def create
    @user = User.new(params[:user])

    if @user.save

      @day_one_counter = []
      @day_one_counter = User.pluck(:time1)

      @day_two_counter = []
      @day_two_counter = User.pluck(:time2)

      b = Hash.new(0)
      c = Hash.new(0)

      @day_one_counter.each do |v|
        b[v] += 1
      end

      @day_two_counter.each do |v|
        c[v] += 1
      end

      @time1 = b["11:30"] 
      @time2 = b["11:45"] 
      @time3 = b["12:00"] 
      @time4 = b["12:15"] 
      @time5 = b["12:30"] 
      @time6 = b["12:45"]
      @time7 = b["1:00"] 
      @time8 = b["1:15"] 
      @time9 = b["1:30"]
      @time10 = b["1:45"] 
      @time11 = b["5:00"] 
      @time12 = b["5:15"]
      @time13 = b["5:30"] 
      @time14 = b["5:45"]
      @time15 = b["6:00"] 
      @time16 = b["6:15"]
      @time17 = b["6:30"] 
      @time18 = b["6:45"] 
      @time19 = b["7:00"]
      @time20 = b["7:15"]

      @time21 = c["11:30"] 
      @time22 = c["11:45"] 
      @time23 = c["12:00"] 
      @time24 = c["12:15"] 
      @time25 = c["12:30"] 
      @time26 = c["12:45"]
      @time27 = c["1:00"] 
      @time28 = c["1:15"] 
      @time29 = c["1:30"]
      @time30 = c["1:45"] 
      @time31 = c["5:00"] 
      @time32 = c["5:15"]
      @time33 = c["5:30"] 
      @time34 = c["5:45"]
      @time35 = c["6:00"] 
      @time36 = c["6:15"]
      @time37 = c["6:30"] 
      @time38 = c["6:45"] 
      @time39 = c["7:00"]
      @time40 = c["7:15"]

      # respond_to do |format|
      #   format.html
      #   format.js
      # end   

      render :time
    else
      render :new
    end
  end

{% endcodeblock %}

<p>This code gets really cumbersome the more time columns I add, so my next step is to DRY this shit up.  But hell, it works and is going to make my friend's life easier, yay! for technology.</p>  