---
layout: post
title: "Changing Your Password from Scratch"
date: 2013-09-07 01:08
comments: true
categories: rails password devise
---

<p> So say you are using devise and have none of the has_secure_password medthod available to you.  One should learn the bcrypt gem... and needs to abstract some methods in order to parse an encrypted password.  Cool, Bcrypt can do that, as located <a href ='  https://github.com/rails/rails/blob/b965ce361b89ad33a4a4b422f8e564233926c723/activemodel/lib/active_model/secure_password.rb#L42
'>here.</a> Here is my modified code in order to confirm if a new password in order to apply a boolean value to an inputed password.</p>

{% codeblock edit.html.haml lang: ruby %}
 %li 
      = f.label :current_password
      = f.password_field :current_password
      # => 'password'
    %li 
      = f.label :new_password, 'New Password'
      = f.password_field :new_password 
      # => 'new_password'
            
  .buttons
    %button Save
{% endcodeblock %}

{% codeblock user.rb lang:ruby %}   
	def authenticate(unencrypted_password)
    if BCrypt::Password.new(encrypted_password) == unencrypted_password && self
      bcrypt = ::BCrypt::Password.new(encrypted_password)
      # creates a bcrypt variable if the encrypted passwors result is true
      # => "$2a$10$wgOzLhy84peHUD9wr9UkgOKRpwfls/0h48NYVvKIOdUdbz3XOEpSK" 
      password = ::BCrypt::Engine.hash_secret(password, bcrypt.salt)
      # then salts the new password
      # => "$2a$10$VUNoD3xdAp7ytTIsTyH5feY.DNUKA4efIdkcI6ViBQ532o8lyNV/e" 
      user = nil unless reset_secure_compare(password, encrypted_password)
      return true 
    else BCrypt::Password.new(encrypted_password) != unencrypted_password && self
      return false
    end
  end
{% endcodeblock %}

 <p>You might be wondering what the reset_secure_password method is doing.  Well, it is pulled from the Devise docs and is preventing <a href='http://en.wikipedia.org/wiki/Timing_attack'>timing attacks,</a> when an attacker attempts to compromise an encryption by analyzing the time taken in order to execute the password and salting algorithms.</p>  

<p>Cool, now I can pass my current_password attribute to make sure it is true.  I need to make a custom <a href='http://edgeguides.rubyonrails.org/active_record_validations.html'>Active Record Validations.</a></p>

{% codeblock user.rb lang:ruby %}
	def correct_password_update_validator
	  if self.authenticate(current_password) == true and current_password.present? and new_password.present?
	    self.encrypted_password = ::BCrypt::Password.create(new_password)
	  end
	 end
{% endcodeblock %}

<p>If this validates, encrypted_password on the database returns true.  Now I need validations for events with blank fields (I want nothing to happen), and also, if current_password is blank and new_password and present, visa versa.</p>

{% codeblock user.rb lang:ruby %}

	validate :incorrect_password_update_validator, on: :update
  validate :correct_password_update_validator, on: :update
  validate :current_password_present, on: :update
  validate :new_password_present, on: :update
  validate :current_password_true_present, on: :update

  private 

	def current_password_true_present
    if self.authenticate(current_password) == true and new_password.blank? and current_password.present?
      errors.add(:new_password, " needs to be filled out.")
    end
  end

  def current_password_present
    if self.authenticate(current_password) == false and new_password.present? and current_password.blank?
      errors.add(:current_password, " needs to be filled out.")
    end
  end

  def new_password_present
    if self.authenticate(current_password) == false and new_password.blank? and current_password.present?
      errors.add(:new_password, " needs to be filled out.")
    end
  end

  def incorrect_password_update_validator
    if self.authenticate(current_password) == false and current_password.present? and new_password.present?
      errors.add(:current_password, " does not match.")
    end
  end

  def reset_secure_compare(a, b)
    return false if a.blank? || b.blank? || a.bytesize != b.bytesize
    l = a.unpack "C#{a.bytesize}"

    res = 0
    b.each_byte { |byte| res |= byte ^ l.shift }
    res == 0
  end
{% endcodeblock %}

<p>Now, all use cases of the user improperly editing the form result in false and a validation error occurs.</p>
