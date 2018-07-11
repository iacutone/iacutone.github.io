---
layout: post
title: Understanding Test Doubles
date: 2017-10-24 12:02:12 -0400
comments: true
categories: rspec ruby rails
---

I refactored credit card payments on an application that uses the `ActiveMerchant` gem. I was not confident in the test, so, I re-wrote the tests to verify responses from Braintree. Once I verified I did not break anything with my refactor, I decided the best next step was to remove the interaction with Braintree and replace it with stubbed responses. I always forget when and how to use RSpec `allow`, `spy` and `double` methods. This post is meant to help reinforce my knowledge of the aforementioned methods.

```ruby
describe CreditCardPayment do
  let!(:payment) { build(:credit_card_payment) }

  describe '#authorize' do
    before { payment.authorize }

    it 'records the GatewayTransaction' do
      transaction = GatewayTransaction.last

      expect(transaction.success).to eq true
      expect(transaction.amount).to eq amount
      expect(transaction.message).to eq '1000 Approved'
      expect(transaction.response).to be_present
      expect(transaction.service_fee).to eq service_fee[:service_fee]
    end
  end
end
```

Authorize is a wrapper around the payment gateway #authorize method which returns a response from Braintree. I should have confidence that when sending Braintree arguments that the`#authorize` expects, the response will be successful. This is interaction is what needs to be stubbed in order to not call the Braintree API in the test environment. The benefits of this approach are that the test will be faster and more resilient because there is no communication with an external service. The negatives of this approach is that the API can change.

Let's update the above codeblock to stub out the interaction with Braintree.

```ruby
describe CreditCardPayment do
  let(:gateway_double) { double('ActiveMerchant::Billing::BraintreeGateway') }
  let!(:payment) { build(:credit_card_payment, gateway: gateway_double) }

  before { payment.gateway = gateway_double }

  describe '#authorize' do
    it 'sends a #authorize request to Braintree' do
      expect(gateway_double).to receive(:authorize).exactly(1).times
        .with(amount, payment.credit_card, payment.credit_card_descriptor)
	.and_return(authorize_response_stub)

      payment.authorize
    end

    it 'creates a GatewayTransaction' do
      expect { payment.authorize }.to change(GatewayTransaction, :count).by(1)
    end
  end

  def authorize_response_stub
    params = { 'response_from_braintree' => 'yay' }

     ActiveMerchant::Billing::Response.new(true, '1000 Approved', params, {authorization: '3e4r5q'})
  end
end
```

Notice that in the `it` block the expectation comes before the `#payment` method call. We could replace the `double` with a `spy`. The difference is a spy uses the `have_received` method. It is your preference to use either a `double` or a `spy`.

```ruby
  let(:gateway_double) { spy('ActiveMerchant::Billing::BraintreeGateway') }
  let!(:payment) { build(:credit_card_payment, gateway: gateway_double) }

  before { payment.gateway = gateway_double }

  describe '#authorize' do
    it 'sends a #authorize request to Braintree' do
      payment.authorize

      expect(gateway_double).to have_receive(:authorize).exactly(1).times
        .with(amount, payment.credit_card, payment.credit_card_descriptor)
	.and_return(authorize_response_stub)
    end

    it 'creates a GatewayTransaction' do
      expect { payment.authorize }.to change(GatewayTransaction, :count).by(1)
    end
  end
```

### Helpful Links
[RSpec Mocks 3.6](https://relishapp.com/rspec/rspec-mocks/v/3-6/docs/basics)
