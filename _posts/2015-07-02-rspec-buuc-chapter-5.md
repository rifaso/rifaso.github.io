---
layout: post
title:  "RSpec Buuc - Chapter 5"
date:   2015-07-02 13:34:39
categories: rspec-buuc ruby rspec
comments: true
---
Chapter 5 needs updating because of the change from Ruby 1.8 to 1.9 and changes from RSpec 2.0 to 3.0.

### Replace Ruby Require with Require Helper

When following the book, example cb/08/spec/codebreaker/game_spec.rb and example cb/08/spec/spec_helper.rb use `require`. Require in Ruby 1.9 no longer makes the current directory part of the `LOAD_PATH`. This changed occured because it was considered a security risk on how require previously worked. 

As the current directory is no longer part of the `LOAD_PATH`, using `require` will give the following error when running RSpec in section 5.1 of the book.

>'require': cannot load such file -- required_file (LoadError)

Using `require_relative` with the relative path will cause the error to go away. 

`require 'spec_helper'` should be replaced with `require_relative '../spec_helper'` 

### Replace Output Due To New RSpec Output Matcher

When trying to run the cumcumber tests in Chapter 5 the following error occurs.

> When I start a new game                     # 15/features/step_definitions/codebreaker_steps.rb:18
> private method `puts' called for #<RSpec::Matchers::BuiltIn::Output:0x000001015e3cb8> (NoMethodError)
> ./15/lib/codebreaker/game.rb:8:in `start'
> ./15/features/step_definitions/codebreaker_steps.rb:20:in `/^I start a new game$/'
> 15/features/codebreaker_starts_game.feature:9:in `When I start a new game'

In `codebreaker_steps.rb` the class `Output` was defined in the book. RSpec 3.0 now has a `output` matcher. The defined class and the ouput matcher are creating the conflict. The quickest way to correct this is to rename our all `output` code to something like `results` and the code will work.

### Chapter 5 Updated Code

RSpec Book Chapter 5 Code that needs to be updated is as follows:

```ruby
cb/08/spec/spec_helper.rb

0 require_relative '../lib/codebreaker'
```

```ruby
cb/08/spec/codebreaker/game_spec.rb

0 require_relative '../spec_helper'
1
2 module Codebreaker
3   describe Game do
4     describe "#start" do
5       it 'sends a welcome message'
6       it 'prompts for the first guess'
7     end
8   end
9 end
```

```ruby
cb/09/spec/codebreaker/game_spec.rb

0  require_relative '../spec_helper'
1
2  module Codebreaker
3    describe Game do
4      describe "#start" do
5        it 'sends a welcome message' do
6          result = double('result')
7          game = Game.new(result)
8
9          expect(result).to receive(:puts).with('Welcome to Codebreaker!')
10
11         game.start
12       end
13
14       it 'prompts for the first guess'
15     end
16   end
17
18 end
```

```ruby
cb/10/lib/codebreaker/game.rb

0  module Codebreaker
1    class Game
2      def initialize(result)
3        @result = result
4      end
5
6      def start
7        @result.puts "Welcome to Codebreaker!"
8      end
9    end
10 end
```

```ruby
cd/11/spec/codebreaker/game_spec.rb

0  require_relative '../spec_helper'
1
2  module Codebreaker
3    describe Game do
4      describe "#start" do
5        it 'sends a welcome message' do
6          result = double('result')
7          game = Game.new(result)
8
9          expect(result).to receive(:puts).with('Welcome to Codebreaker!')
10
11         game.start
12       end
13
14       it 'prompts for the first guess' do
15         result = double('result')
16         game = Game.new(result)
17
18         expect(result).to receive(:puts).with('Enter guess:')
19
20         game.start
21       end
22     end
23   end
24
25 end
```

```ruby
cb/12/lib/codebreaker/game.rb
0  module Codebreaker
1    class Game
2      def initialize(result)
3        @result = result
4      end
5
6      def start
7        @result.puts "Welcome to Codebreaker!"
8        @result.puts "Enter guess:"
9      end
10   end
11 end
```

```ruby
cd/14/spec/codebreaker/game_spec.rb

0  require_relative '../spec_helper'
1
2  module Codebreaker
3    describe Game do
4      describe "#start" do
5        before(:each) do
6          @result = double('result').as_null_object
7          @game = Game.new(result)
8        end
9
10       it 'sends a welcome message' do
11         expect(@result).to receive(:puts).with('Welcome to Codebreaker!')
12         @game.start
13       end
14
15       it 'prompts for the first guess' do
16         expect(@result).to receive(:puts).with('Enter guess:')
17         @game.start
18       end
19     end
20   end
21
22 end
```

```ruby
cd/15/spec/codebreaker/game_spec.rb
0  require_relative '../spec_helper'
1
2  module Codebreaker
3    describe Game do
4      describe "#start" do
5
6        let(:result) { double('result').as_null_object }
7        let(:game) { Game.new(result) }
8
9        it 'sends a welcome message' do
10          expect(result).to receive(:puts).with('Welcome to Codebreaker!')
11          game.start
12       end
13
14       it 'prompts for the first guess' do
15         expect(result).to receive(:puts).with('Enter guess:')
16         game.start
17       end
18     end
19   end
20
21 end
```

#Additional Updated Code

The following code from previous chapters has been updated because (1) it no longer works by the end of Chapter 5 or (2) renaming `output` to `results` for consistency in code base.

```ruby
cb/04/lib/codebreaker.rb

0 require_relative 'codebreaker/game'
```

```ruby
cb/06/features/step_definitions/codebreaker_steps.rb
0  class Result
1    def messages
2      @messages ||= []
3    end
4
5    def puts(message)
6      messages << message
7    end
8  end
9
10 def result
11   @result ||= Result.new
12 end
13
14 Given(/^I am not yet playing$/) do
15 end
16
17 When(/^I start a new game$/) do
18   game = Codebreaker::Game.new(result)
19   game.start
20 end
21
22 Then(/^I should see "([^"]*)"$/)do |message|
23   expect(result.messages).to include(message)
24 end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
