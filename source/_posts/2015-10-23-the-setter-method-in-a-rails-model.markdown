---
layout: post
title: The Setter Method in a Rails Model
date: 2015-10-23 15:30:53 -0500
comments: true
categories: ruby rails
---

At Fractured Atlas, we are trying to move away from the additional complexity of using callbacks. For example, let's say we need to do something in a before_save callback such as lowercase a user's email.

```ruby user.rb

before_save: lowercase_email

def lowercase_email
  self.email = self.email.downcase
end

```

How is this code implemented without using before_save? One method would be overriding the setter accessor.

```ruby user.rb

def email=(value)
 email = value.downcase
 super(email)
end

```

Now, when the email attribute is updated, my email setter method is called.

Currently, both implementations for downcasing an email are straightforward. However, as the application grows, the before_save callback is more difficult to test and maintain. The before_save is implicit and might have unexpected side effects. Here is a basic spec.

```ruby user_spec.rb

describe User do
  
  let(:user) { FactoryGirl.create(:user) }

  describe '#lowercase_email' do
    before { user.email = 'GNARLY@email.com' }

    it 'lowercases the email' do
      expect(user.lowercase_email).to eq 'gnarly@email.com'
    end
  end
end

```

Instead, let's test the *behavior* of what we expect.

```ruby user_creation_spec.rb

describe 'User Creation' do
  let(:user) { FactoryGirl.create(:user) }

  describe ':email' do
    before { user.email = 'GNARLY@email.com' }

    'it returns a standardized email' do
      expect(user.email).to eq 'gnarly@email.com'
    end
  end
end

```

The first spec is directly testing the callback, whereas the latter spec is testing the *behavior* of the expected outcome. The latter spec is easier to reason about and change. This is an easy example, but as callbacks grow changing their behavior becomes more difficult. This is especially true if the callbacks are coupled with other models. 

*Helpful Links*

[What is the right way to override a setter method in Ruby on Rails?](http://stackoverflow.com/questions/10464793/what-is-the-right-way-to-override-a-setter-method-in-ruby-on-rails)

[Overwriting default accessors](http://api.rubyonrails.org/classes/ActiveRecord/Base.html#class-ActiveRecord%3a%3aBase-label-Overwriting+default+accessors) 
