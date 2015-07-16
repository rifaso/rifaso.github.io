---
layout: post
title: 'RSpec Buuc - Chapter 20'
date: 2015-07-16 12:52:41
categories: rspec-buuc
comments: true
---

Chapter 20 needs only one change due to updating to the `:expect` syntax. See [RSpec Buuc - Chapter 2][ch2] for more details.

```ruby
cucumber_rails/02/features/step_definitions/showtime_steps.rb

Given(/^a movie$/) do
  @movie = Movie.create!
end

When(/^I set the showtime "(.*?)" at "(.*?)"$/) do |date, time|
  @movie.update_attribute(:showtime_date, Date.parse(date))
  @movie.update_attribute(:showtime_time, time)
end

Then(/^the showtime description should be "(.*?)"$/) do |showtime|
  expect(@movie.showtime).to eq(showtime)
end

```
All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[ch2]:/rspec-buuc/2015/06/30/rspec-buuc-chapter-2.html
[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
