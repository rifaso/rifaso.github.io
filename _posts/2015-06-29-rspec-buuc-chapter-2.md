---
layout: post
title:  "RSpec Buuc - Chapter 2"
date:   2015-06-30 14:23:39
categories: rspec-buuc
comments: true
---
Chapter 2 has the first piece of code that needs updating. RSpec is depreciating the `:should` syntax in favor of the `:expect` syntax.

`greeting.should == "Hello RSpec!"`

Will give the message: 

>Deprecation Warnings:
>
>Using 'should' from rspec-expectations' old ':should' syntax without explicitly enabling the syntax is deprecated. Use the new ':expect' syntax or explicitly enable ':should' with 
>'config.expect_with(:rspec) { |c| c.syntax = :should }' instead. 
>
>If you need more of the backtrace for any of these deprecations to identify where to make the necessary changes, you can configure 'config.raise_errors_for_deprecations!', and it will turn the deprecation warnings into errors, giving you the full backtrace.

With the new expect syntax we rewrite `greeting.should == "Hello RSpec!"` with `expect(greeting).to eq("Hello RSpec")` and we don't get any warnings.

You may note that we are also using `eq()` instead of `==`. The [RSpec blog][rspec-blog] inditcates that == will give the following warning if warnings are turned on:

>"Useless use of == in void context" 

Using the expect syntax and eq(), updates RSpec Book Chapter 2 Code as follows.

```ruby
hello/1/greeter_spec.rb

0 describe "RSpec Greeter" do
1   it "should say  'Hello RSpec!' when it recieves the greet() message" do
2     greeter = RSpecGreeter.new
3     greeting = greeter.greet
4     expect(greeting).to eq("Hello RSpec!")
5   end
6 end     
```

```ruby
hello/2/greeter_spec.rb

0  class RSpecGreeter
1    def greet
2      "Hello RSpec!"
3    end
4  end
5
6  describe "RSpec Greeter" do
7    it "should say  'Hello RSpec!' when it recieves the greet() message" do
8      greeter = RSpecGreeter.new
9      greeting = greeter.greet
10     expect(greeting).to eq("Hello RSpec!")
11   end
12 end
```

```ruby
hello/4/features/step_definitions/greeter_steps.rb

0  Given /^a greeter$/ do
1    @greeter = CucumberGreeter.new
2  end
3
4  When /^I send it the greet message$/ do
5    @message = @greeter.greet
6  end
7
8  Then /^I should see "([^"]*)"$/ do |greeting|
9    @message.should == greeting
10 end 
```

```ruby
hello/5/features/step_definitions/greeter_steps.rb

0  class CucumberGreeter
1    def greet
2      "Hello Cucumber!"
3    end
4  end
5
6  Given /^a greeter$/ do
7    @greeter = CucumberGreeter.new
8  end
9
10 When /^I send it the greet message$/ do
11   @message = @greeter.greet
12 end
13
14 Then /^I should see "([^"]*)"$/ do |greeting|
15   expect(@message).to eq(greeting)
16 end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[rspec-blog]: http://rspec.info/blog/2012/06/rspecs-new-expectation-syntax/#fn:foot_1
[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
