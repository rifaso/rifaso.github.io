---
layout: post
title: 'Project Euler - Problem 1'
date: 2015-07-15 14:32:49
categories: project-euler
comments: true
---
# Problem 1 - Multiples of 3 and 5

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.

# How I Solved It

I first solved for finding multiples of three. Then I solved for multiples of five. These are the two methods I wrote.

```ruby
def initialize(max_number)
  @range = 1...max_number
  @result = []
end

def multiples_of_three
  @range.each do |number|
    @result << number if number % 3 == 0
  end
  @result
end

def multiples_of_five
  @range.each do |number|
    @result << number if number % 5 == 0
  end
  @result
end
```

I refactor this as:

```ruby
def multiples_of(multiple)
  @range.each do |number|
    @result << number if number % multiple == 0
  end
  @result
end
```

This worked well until I wanted to solve for multiples of 3 and 5 so I modified the code to this:

```ruby
def multiples_of(*multiples)
  multiples.each do |multiple|
    @range.each do |number|
      @result << number if number % multiple == 0
    end
  end
  @result.sort
end
```

I then sovled for the sum of these multiples.

```ruby
def sum_of_multiples_of(*multiples)
  multiples_of(*multiples)
  @result.reduce(:+)
end
```

Everything looked great, all my tests were passing. Ran it for the sum of all the multiples of 3 and 5 below 1000. Submitted the answer to Project Euler and failed.

After some investigating found out that I was getting duplicate results in my array when I ran `multiples_of(3, 5)`. For example if the max number is 15 the output array was `[3, 5, 6, 9, 10, 12, 15, 15]` instead of `[3, 5, 6, 9, 10, 12, 15]`. This was quickly using ruby's `#uniq` method.

```ruby
def multiples_of(*multiples)
  multiples.each do |multiple|
    @range.each do |number|
      @result << number if number % multiple == 0
    end
  end
  @result.sort.uniq
end
```

After this my answer worked.

I will be posting all my code and tests at this [GitHub repo][gh-pe].

[gh-pe][https://github.com/mlongerich/project_euler]
