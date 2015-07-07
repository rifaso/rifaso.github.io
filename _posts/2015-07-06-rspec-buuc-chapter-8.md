---
layout: post
title:  "RSpec Buuc - Chapter 8"
date:   2015-07-06 22:48:39
categories: rspec-buuc
comments: true
---
Chapter 8 tells us to run `rspec spec/codebreaker/marker_spec.rb --format nested` which gives us the error:

>'find_formatter': Formatter 'nested' unknown - maybe you meant 'documentation' or 'progress'?. (ArgumentError)

The `nested formatter` was introduced in RSpec 1, in RSpec 2 it was renamed to the `documentation formatter`. Running the following 

`rspec spec/codebreaker/marker_spec.rb --format doc` 

achives the same output as in the book.

Chapter 8 needs only minor updates due to reasons previously discussed in [RSpec Buuc - Chapter 2][ch2] and [RSpec Buuc - Chapter 5][ch5]. Specifically, updating to the `:expect` syntax, and changing code refering to 'output' to 'result' in order to avoid conflicts with RSpec's `output` matcher.

### Chapter 8 Updated Code

```ruby
cb/39/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      exact_match_count = 0
      mark = ''
      (0..3).each do |index|
        if exact_match?(guess, index)
          exact_match_count +=1
        end
      end
      (0..3).each do |index|
        if number_match?(guess, index)
          mark << '-' 
        end
      end
      @result.puts '+'*exact_match_count + mark
    end

    def exact_match?(guess, index)
      guess[index] == @secret[index]
    end

    def number_match?(guess, index)
      @secret.include?(guess[index]) && !exact_match?(guess, index)
    end
  end
end
```

```ruby
cb/391/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      exact_match_count = 0
      number_match_count = 0
      (0..3).each do |index|
        if exact_match?(guess, index)
          exact_match_count +=1
        end
      end
      (0..3).each do |index|
        if number_match?(guess, index)
          number_match_count +=1
        end
      end
      @result.puts '+'*exact_match_count + '-'*number_match_count
    end

    def exact_match?(guess, index)
      guess[index] == @secret[index]
    end

    def number_match?(guess, index)
      @secret.include?(guess[index]) && !exact_match?(guess, index)
    end
  end
end
```

```ruby
cb/393/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      number_match_count = 0
      (0..3).each do |index|
        if number_match?(guess, index)
          number_match_count +=1
        end
      end
      @result.puts '+'*exact_match_count(guess) + '-'*number_match_count
    end

    def exact_match?(guess, index)
      guess[index] == @secret[index]
    end

    def exact_match_count(guess)
      exact_match_count = 0
      (0..3).each do |index|
        if exact_match?(guess, index)
          exact_match_count += 1
        end
      end
      exact_match_count
    end

    def number_match?(guess, index)
      @secret.include?(guess[index]) && !exact_match?(guess, index)
    end
  end
end
```

```ruby
cb/394/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      @result.puts '+'*exact_match_count(guess) + '-'*number_match_count(guess)
    end

    def exact_match?(guess, index)
      guess[index] == @secret[index]
    end

    def exact_match_count(guess)
      exact_match_count = 0
      (0..3).each do |index|
        if exact_match?(guess, index)
          exact_match_count += 1
        end
      end
      exact_match_count
    end

    def number_match?(guess, index)
      @secret.include?(guess[index]) && !exact_match?(guess, index)
    end

    def number_match_count(guess)
      number_match_count = 0
      (0..3).each do |index|
        if number_match?(guess, index)
          number_match_count += 1
        end
      end
      number_match_count
    end
  end
end
```

```ruby
cb/411/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      marker = Marker.new(@secret)
      @result.puts '+'*marker.exact_match_count(guess) + 
                   '-'*marker.number_match_count(guess)
    end
  end

  class Marker
    def initialize(secret)
      @secret = secret
    end

    def exact_match?(guess, index)
      guess[index] == @secret[index]
    end

    def exact_match_count(guess)
      (0..3).inject(0) do |count, index|
        count + (exact_match?(guess, index) ? 1 : 0)
      end
    end

    def number_match?(guess, index)
      @secret.include?(guess[index]) && !exact_match?(guess, index)
    end

    def number_match_count(guess)
      (0..3).inject(0) do |count, index|
        count +(number_match?(guess, index) ? 1 : 0)
      end
    end
  end
end
```

```ruby
cb/414/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @secret = secret
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      marker = Marker.new(@secret, guess)
      @result.puts '+'*marker.exact_match_count + 
                   '-'*marker.number_match_count(guess)
    end
  end

  class Marker
    def initialize(secret, guess)
      @secret, @guess = secret, guess
    end

    def exact_match?(guess, index)
      @guess[index] == @secret[index]
    end

    def exact_match_count
      (0..3).inject(0) do |count, index|
        count + (exact_match?(@guess, index) ? 1 : 0)
      end
    end

    def number_match?(guess, index)
      @secret.include?(@guess[index]) && !exact_match?(@guess, index)
    end

    def number_match_count(guess)
      (0..3).inject(0) do |count, index|
        count +(number_match?(@guess, index) ? 1 : 0)
      end
    end
  end
end
```

```ruby
cb/42/spec/codebreaker/marker_spec

require_relative '../spec_helper'

module Codebreaker
  describe Marker do
    describe "#exact_match_count" do
      context "with no matches" do
        it "returns 0" do
          marker = Marker.new('1234', '5555')
          expect(marker.exact_match_count).to eq(0)
        end
      end

      context "with 1 exact match" do
        it "returns 1" do
          marker = Marker.new('1234', '1555')
          expect(marker.exact_match_count).to eq(1)
        end
      end

      context "with 1 number match" do
        it "returns 0" do
          marker = Marker.new('1234', '2555')
          expect(marker.exact_match_count).to eq(0)
        end
      end

      context "with 1 exact match and 1 number match" do
        it "returns 1" do
          marker = Marker.new('1234', '1525')
          expect(marker.exact_match_count).to eq(1)
        end
      end
    end

    describe "#number_match_count" do
      context "with no matches" do
        it "returns 0" do
          marker = Marker.new('1234', '5555')
          expect(marker.number_match_count).to eq(0)
        end
      end

      context "with 1 exact match" do
        it "returns 0" do
          marker = Marker.new('1234', '1555')
          expect(marker.number_match_count).to eq(0)
        end
      end

      context "with 1 number match" do
        it "returns 1" do
          marker = Marker.new('1234', '2555')
          expect(marker.number_match_count).to eq(1)
        end
      end

      context "with 1 exact match and 1 number match" do
        it "returns 1" do
          marker = Marker.new('1234', '1525')
          expect(marker.number_match_count).to eq(1)
        end
      end
    end
  end
end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[ch2]:/rspec-buuc/2015/06/30/rspec-buuc-chapter-2.html
[ch5]:/rspec-buuc/2015/07/02/rspec-buuc-chapter-5.html
[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
