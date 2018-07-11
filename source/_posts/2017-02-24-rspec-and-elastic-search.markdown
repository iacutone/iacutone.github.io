---
layout: post
title: RSpec and Elastic Search
date: 2017-02-24 11:39:11 -0500
comments: true
categories: rails rspec elasticsearch
---

I had a difficult time setting up  ElasticSearch on both RSpec (CircleCI) and Heroku. The ElasticSearch test cluster was not working on the CircleCI Docker image. Fortunately, one can configure the Circle environment to start with an ElasticSearch process. So, instead of using the test cluster, both my local testing environment and Circle environment use a real ElasticSearch process.

```ruby
#
# ElasticSearch
#

config.before :all, elasticsearch: true do
  port = ENV['CIRCLE_CI_ES_URL'].present? ? 9200 : 9250
  Elasticsearch::Model.client = Elasticsearch::Client.new(port: port)
end

config.before :each, elasticsearch: true do
  Campaign.__elasticsearch__.create_index!(force: true)
end

config.after :each, elasticsearch: true do
  Campaign.__elasticsearch__.delete_index!
end
```

When a test needs to use ElasticSearch:

```ruby
before { Campaign.__elasticsearch__.refresh_index! }

describe '#search', elasticsearch: true do
  expect(search.results).to be_present
end
```

I ran into issues using ElasticSearch on Heroku when creating an index. Heroku review apps are configurable by defining an `app.json`. In the json file, Heroku can spin up an ElasticSearch process.

```
"scripts": {
  "postdeploy": "bundle exec rake db:schema:load db:seed"
},
"formation": {
  "web": {
    "size": "free",
    "quantity": 1
  },
  "elasticsearch": {
    "size": "free",
    "quantity": 1
  }
}
```

The process is running before any Ruby code is executed. The next step is to create the index using `postdeploy`. Before creating ActiveRecord objects in the `seed.rb` file, create an index with `Model.index(force: true)`.
