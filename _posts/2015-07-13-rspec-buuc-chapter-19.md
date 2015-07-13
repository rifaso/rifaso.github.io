---
layout: post
title: 'RSpec Buuc - Chapter 19'
date: 2015-07-13 11:19:35
categories: rspec-buuc
comments: true
---

In Chapter 19 we will only be modifying the gemfile in order to use the most recent versions of rails, rspec-rails, cucumber-rails, and webrat.

```ruby
source 'https://rubygems.org'

gem 'rails', '4.2.3'
gem 'sqlite3'
gem 'sass-rails', '~> 5.0'
gem 'uglifier', '>= 1.3.0'
gem 'coffee-rails', '~> 4.1.0'

gem 'jquery-rails'
gem 'turbolinks'
gem 'jbuilder', '~> 2.0'
gem 'sdoc', '~> 0.4.0', group: :doc

group :development, :test do
  gem 'byebug'
  gem 'web-console', '~> 2.0'
  gem 'spring'
  gem 'rspec-rails'
  gem 'cucumber-rails'
  gem 'webrat'
end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
