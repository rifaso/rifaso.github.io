---
layout: post
title:  "RSpec Buuc - Chapter 7"
date:   2015-07-05 23:48:39
categories: rspec-buuc
comments: true
---
Chapter 7 needs only minor updates due to reasons previously discussed in [RSpec Buuc - Chapter 2][ch2] and [RSpec Buuc - Chapter 5][ch5]. Specifically, updating to the `:expect` syntax, and changing code refering to 'output' to 'result' in order to avoid conflicts with RSpec's `output` matcher.

### Chapter 6 Updated Code 

```ruby
cb/24/spec/codebreaker/game_spec.rb

require_relative '../spec_helper'

module Codebreaker
  describe Game do
    let(:result) { double('result').as_null_object }
    let(:game) { Game.new(result) }
    
    describe "#start" do


      it 'sends a welcome message' do
        expect(result).to receive(:puts).with('Welcome to Codebreaker!')
        game.start('1234')
      end

      it 'prompts for the first guess' do
        expect(result).to receive(:puts).with('Enter guess:')
        game.start('1234')
      end
    end

    describe "#guess" do
      context "with no matchers" do
        it "sends a mark with ''" do
          game.start('1234')
          expect(result).to receive(:puts).with('')
          game.guess('5555')
        end
      end
    end
  end
end
```

```ruby
cb/25/lib/codebreaker/game.rb

module Codebreaker
  class Game
    def initialize(result)
      @result = result
    end

    def start(secret)
      @result.puts "Welcome to Codebreaker!"
      @result.puts "Enter guess:"
    end

    def guess(guess)
      @result.puts '' 
    end
  end
end
```

```ruby
cb/26/spec/codebreaker/game_spec.rb

require_relative '../spec_helper'

module Codebreaker
  describe Game do
    let(:result) { double('result').as_null_object }
    let(:game) { Game.new(result) }
    
    describe "#start" do


      it 'sends a welcome message' do
        expect(result).to receive(:puts).with('Welcome to Codebreaker!')
        game.start('1234')
      end

      it 'prompts for the first guess' do
        expect(result).to receive(:puts).with('Enter guess:')
        game.start('1234')
      end
    end

    describe "#guess" do
      context "with no matchers" do
        it "sends a mark with ''" do
          game.start('1234')
          expect(result).to receive(:puts).with('')
          game.guess('5555')
        end
      end
      context "with 1 number match" do
        it "sends a mark with '-'" do
          game.start('1234')
          expect(result).to receive(:puts).with('-')
          game.guess('2555')
        end
      end
    end
  end
end
```

```ruby
cb/27/lib/codebreaker/game.rb

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
      if @secret.include?(guess[0])
        @result.puts '-' 
      else
        @result.puts ''
      end
    end
  end
end
```

```ruby
cb/28/lib/codebreaker/game.rb

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
      if @secret.include?(guess[0])
        mark = '-' 
      else
        mark = ''
      end
      @result.puts mark
    end
  end
end
```

```ruby
cb/30/spec/codebreaker/game_spec.rb

require_relative '../spec_helper'

module Codebreaker
  describe Game do
    let(:result) { double('result').as_null_object }
    let(:game) { Game.new(result) }
    
    describe "#start" do


      it 'sends a welcome message' do
        expect(result).to receive(:puts).with('Welcome to Codebreaker!')
        game.start('1234')
      end

      it 'prompts for the first guess' do
        expect(result).to receive(:puts).with('Enter guess:')
        game.start('1234')
      end
    end

    describe "#guess" do
      context "with no matchers" do
        it "sends a mark with ''" do
          game.start('1234')
          expect(result).to receive(:puts).with('')
          game.guess('5555')
        end
      end
      context "with 1 number match" do
        it "sends a mark with '-'" do
          game.start('1234')
          expect(result).to receive(:puts).with('-')
          game.guess('2555')
        end
      end
      context "with 1 exact match" do
        it "sends a mark with '+'" do
          game.start('1234')
          expect(result).to receeive(:puts).with('+')
          game.guess('1555')
        end
      end
    end
  end
end
```

```ruby
cb/33/spec/codebreaker/game_spec.rb

require_relative '../spec_helper'

module Codebreaker
  describe Game do
    let(:result) { double('result').as_null_object }
    let(:game) { Game.new(result) }
    
    describe "#start" do


      it 'sends a welcome message' do
        expect(result).to receive(:puts).with('Welcome to Codebreaker!')
        game.start('1234')
      end

      it 'prompts for the first guess' do
        expect(result).to receive(:puts).with('Enter guess:')
        game.start('1234')
      end
    end

    describe "#guess" do
      context "with no matchers" do
        it "sends a mark with ''" do
          game.start('1234')
          expect(result).to receive(:puts).with('')
          game.guess('5555')
        end
      end
      context "with 1 number match" do
        it "sends a mark with '-'" do
          game.start('1234')
          expect(result).to receive(:puts).with('-')
          game.guess('2555')
        end
      end
      context "with 1 exact match" do
        it "sends a mark with '+'" do
          game.start('1234')
          expect(result).to receive(:puts).with('+')
          game.guess('1555')
        end
      end
      context "with 2 number matches" do
        it 'sends a mark with "--"' do
          game.start('1234')
          expect(result).to receive(:puts).with('--')
          game.guess('2355')
        end
      end
    end
  end
end
```

```ruby
cb/35/spec/codebreaker/game_spec.rb

require_relative '../spec_helper'

module Codebreaker
  describe Game do
    let(:result) { double('result').as_null_object }
    let(:game) { Game.new(result) }
    
    describe "#start" do


      it 'sends a welcome message' do
        expect(result).to receive(:puts).with('Welcome to Codebreaker!')
        game.start('1234')
      end

      it 'prompts for the first guess' do
        expect(result).to receive(:puts).with('Enter guess:')
        game.start('1234')
      end
    end

    describe "#guess" do
      context "with no matchers" do
        it "sends a mark with ''" do
          game.start('1234')
          expect(result).to receive(:puts).with('')
          game.guess('5555')
        end
      end
      context "with 1 number match" do
        it "sends a mark with '-'" do
          game.start('1234')
          expect(result).to receive(:puts).with('-')
          game.guess('2555')
        end
      end
      context "with 1 exact match" do
        it "sends a mark with '+'" do
          game.start('1234')
          expect(result).to receive(:puts).with('+')
          game.guess('1555')
        end
      end
      context "with 2 number matches" do
        it 'sends a mark with "--"' do
          game.start('1234')
          expect(result).to receive(:puts).with('--')
          game.guess('2355')
        end
      end
      context "with 1 number match and 1 exact match (in that order)" do
        it 'sends a mark with "+-"' do
          game.start('1234')
          expect(result).to receive(:puts).with('+-')
          game.guess('2535')
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
