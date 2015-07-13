---
layout: post
title: 'RSpec Buuc - Chapter 19'
date: 2015-07-13 11:19:35
categories: rspec-buuc
comments: true
---

In Chapter 19 we will only be modifying the gemfile in order to use the most recent versions of rails, rspec-rails, cucumber-rails, and capybara. With regards to Capybara, the RSpec book uses Webrat however Cucumber dropped support for it in v0.5, hence the change.

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
  gem 'capybara'
end

group :test do
  gem 'cucumber-rails', :require => false
  gem 'database_cleaner'
end
```

After updating the Gemfile, `bundle install` to install the gems. To then install RSpec and Cucumber do the following:

`$ rails generate rspec:install`
`$ rails generate cucumber:install`

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
