---
layout: post
title: "RSpec Example Blocks"
date: 2016-01-11 15:57:05 -0500
comments: true
categories: rspec rails ruby
---

I recently discovered an interesting test pattern; defining variables in before and after hooks in the rails helper.

``` ruby rails_helper.rb
  config.before(:example, hook: true) do
    @hook = 'before hook'
  end
```

``` ruby some_spec.rb
  RSpec.describe "some variable I need" do

    describe 'my hook', hook: true do
      it 'runs the hook' do
        expect(@hook).to eq 'before hook' # returns true
      end
    end
  end
```

### A Real Example

Let's say we are testing a controller and want to make sure only an admin has access to specific views.

``` ruby rails_helper.rb
  config.before(:example, admin_signed_in: true) do
    current_user_double = Test::User.new :admin => true
    allow_any_instance_of(ApplicationController).to receive(:current_user).and_return(current_user_double)
  end

  config.before(:example, user_signed_in: true) do
    current_user_double = Test::User.new :admin => false
    allow_any_instance_of(ApplicationController).to receive(:current_user).and_return(current_user_double)
  end
```

When setting admin_signed_in to true in a describe block, we have access to an admin user. I think it is cleaner to set up these users in a hook instead of a shared context or explicitly in the spec.

Helpful Links

[Hooks](https://www.relishapp.com/rspec/rspec-core/docs/hooks/filters)
