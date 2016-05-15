---
layout: post
title: "Using Mocks to Test Mailers in RSpec"
date: 2015-12-12 13:37:18 -0500
comments: true
categories: rails ruby rspec
---

I was in the situation where I needed to test the delivery of emails based on specific sets of events. I began testing by inspecting sent emails in ActionMailer::Base.deliveries. However, this method of testing felt verbose and sloppy.

RSpec mocks is a great solution when you need to test that a specific method was called. In my case, I want a fundraiser to receive an email after a campaign goal is reached.

``` ruby donation_funding.rb

class DonationFunding
  attr_reader :donation

  def initialize(donation)
    @donation = donation
  end

  def fund
    # do some work
    CampaignMailer.notify_curator_campaign_goal_reached(donation)
  end
end

```

``` ruby donation_funding_spec.rb

  before do
    donation.amount = campaign.goal_amount + 1
    donation.save!
  end

  # Make a donation that exceeds the funding goal

  let!(:funder) {
    funder = DonationFunding.new donation
    funder
  }

  # set up the donation for funding

  it 'sends email goal amount reached email to the curator' do
    expect(CampaignMailer).to receive(:notify_curator_campaign_goal_reached).and_call_original
    funder.fund
  end

  # Expect a CampaignMailer.notify_curator_campaign_goal_reached email to be sent

```

The definition for the method and_call_original is, "You can use and_call_original on the fluent interface to "pass through" the received message to the original method." What exactly does this mean? It means that when I call funder.fund, I expect CampaignMailer.notify_curator_campaign_goal_reached to be called.


Helpful Links

[Calling the original method](https://www.relishapp.com/rspec/rspec-mocks/v/2-14/docs/message-expectations/calling-the-original-method)

