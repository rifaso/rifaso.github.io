---
layout: post
title:  "RSpec Buuc - Chapter 9"
date:   2015-07-07 11:41:39
categories: rspec-buuc
comments: true
---

Chapter 9 needs only one change due to updating to the `:expect` syntax. See [RSpec Buuc - Chapter 2][ch2] for more details.

### Chapter 9 Updated Code

```ruby
cb/43/spec/codebreaker/marker_spec.rb

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

      context "with 1 exact match duplicates in guess" do
        it "returns 0" do
          marker = Marker.new('1234', '1155')
          expect(marker.number_match_count).to eq(0)
        end
      end
    end
  end
end
```

All code for the RSpec Buuc can be found at this [GitHub repo][rspec-buuc-repo].

[ch2]:/rspec-buuc/2015/06/30/rspec-buuc-chapter-2.html
[rspec-buuc-repo]: https://github.com/mlongerich/rspec_buuc
