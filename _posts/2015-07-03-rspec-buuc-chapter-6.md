---
layout: post
title:  "RSpec Buuc - Chapter 6"
date:   2015-07-03 00:34:39
categories: rspec-buuc
comments: true
---
Chapter 6 needs only minor updates due to reasons previously discussed in [RSpec Buuc - Chapter 2][ch2] and [RSpec Buuc - Chapter 5][ch5]. Specifically, updating to the `:expect` syntax, and changing code refering to 'output' to 'result' in order to avoid conflicts with RSpec's `output` matcher.

### Chapter 6 Updated Code 

```ruby
cb/16/features/step_definitions/codebreaker_steps.rb

0  class Result
1    def messages
2      @messages ||= []
3
4    end
5
6    def puts(message)
7      messages << message
8    end
9  end
10
11 def result
12   @result ||= Result.new
13 end
14
15 Given(/^I am not yet playing$/) do
16 end
17
18 When(/^I start a new game$/) do
19   game = Codebreaker::Game.new(result)
20   game.start
21 end
22
23 Then(/^I should see "([^"]*)"$/)do |message|
24   expect(result.messages).to include(message)
25 end
26
27 Given(/^the secret code is "([^"]*)"$/) do |secret|
28   game = Codebreaker::Game.new(result)
29   game.start(secret)
30 end
```

```ruby
cb/22/features/step_definitions/codebreaker_steps.rb

1  class Result
2    def messages
3      @messages ||= []
4    end
5
6    def puts(message)
7      messages << message
8    end
9  end
10
11 def result
12   @result ||= Result.new
13 end
14
15 Given(/^I am not yet playing$/) do
16 end
17
18 When(/^I start a new game$/) do
19   game = Codebreaker::Game.new(result)
20   game.start('1234')
21 end
22
23 Then(/^I should see "([^"]*)"$/)do |message|
24   expect(result.messages).to include(message)
25 end
26
27 Given(/^the secret code is "([^"]*)"$/) do |secret|
28   @game = Codebreaker::Game.new(result)
29   @game.start(secret)
30 end
31
32 When(/^I guess "([^"]*)"$/) do |guess|
33   @game.guess(guess) 
34 end 
35
36 Then(/^the mark should be "([^"]*)"$/) do |mark|
37   expect(result.messages).to include(mark)
38 end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[ch2]:/rspec-buuc/2015/06/30/rspec-buuc-chapter-2.html
[ch5]:/rspec-buuc/2015/07/02/rspec-buuc-chapter-5.html
[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
