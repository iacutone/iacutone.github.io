---
layout: post
title: "Shared Contexts with RSpec"
date: 2015-11-20 16:45:46 -0500
comments: true
categories: rspec ruby rails
---

In the past, I used traits with my factories. Below is a simple example of defining a trait:

### Using traits
```ruby donations_factories.rb
  FactoryGirl.define do
    factory :donation do
      trait :with_amount do
        amount 10
      end

      trait :no_amount do
        amount 0
      end
    end
  end
```

An example spec:

```ruby donation_spec.rb
  RSpec.describe 'Donation Matching', :type => :model do

    context 'with an amount' do
      let(:donation) { create(:donation, :with_amount) }
    end

    context 'with no amount' do
      let(:donation) { create(:donation, :no_amount) }
    end
  end
```

However, I believe that using a shared context is a clearer approach. Defining the amount in a shared context and overriding the amount allows more control over testing use cases.

### Using a shared context
```ruby donation_spec.rb
  require 'support/shared_contexts/donor_with_donation_context'

  RSpec.describe 'Donation Matching', :type => :model do
    include_context 'a donor with a donation'

    context 'with an amount' do
      // donation.amount == 10 (defined in the shared context)
    end

    context 'with no amount' do
      let(:amount) { 0 }
      // donation.amount == 0
    end

    context 'with a gnarly' do
      let(:amount) { 09709780 }
    end
  end
```

```ruby spec/support/shared_contexts/donor_with_donation_context.rb
  RSpec.shared_context 'a donor with a donation' do
    let(:amount) { 10 }

    let(:donation) { create(:donation, amount: amount) }
  end
```

I especially like the shared context approach because you can have shared context and redefine attributes as needed in specs.
