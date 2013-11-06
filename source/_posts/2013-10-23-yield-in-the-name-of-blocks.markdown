---
layout: post
title: "Yield! In the Name of Blocks"
date: 2013-10-23 08:38
comments: true
categories: ruby
published: true
---

Let's think it <a href="http://www.metrolyrics.com/stop-in-the-name-of-love-lyrics-the-supremes.html" target="_blank">over</a>...

In an earlier <a href="http://gregeng.github.io/blog/2013/10/09/abstraction-and-3-iterators/" target="_blank">post</a>, I talked about a few Ruby iterators and how they abstract to become useful tools. As promised, I'm going to further break them down in order to describe the conept of `yield` in Ruby.

In Ruby, methods are functions that allow code to interact with all different pieces of the program. Compared to spoken languages, you can think of methods as the verbs which connect to nouns and other words to form sentences. Methods are able to receive a code `block` which unlocks limitless potential.
When defining our own methods, the magic of `yield` comes into play. If a method expects a block, it can invoke it by using `yield`.

For example:

```ruby

class Weather
    def initialize(type)
         @type = type
    end

    def forecast
        yield(@type)
    end
end

today = Weather.new("hot")

```
Here I am defining a weather class that takes an argument at instantiation. I've also defined a method `forecast` that will take any block given and pass it the argument.

```ruby

today.forecast do |type|
  puts "It's going to be #{type}!"
end

# => It's going to be hot!

```
As you can see, I've interpolated the variable with a friendly little forecast sentence.

But I could just as easily use a different block to perform a different function.

```ruby

today.forecast do |type|
  puts "It's going to be #{type.upcase}, #{type.upcase}, #{type.upcase}!"
end

# => It's going to be HOT, HOT, HOT!

```

In this case... I am loudly saying it's going to be *fairly* warm.

Building on this idea, you can see that it essentially allows any method to easily become an iterator. Here is another look at the iterators described in my previous post: `each`, `collect`, `select`.


`each` dissected:

```ruby
def my_each(array)
  i = 0
  while i < array.length
    yield(array[i])
    i += 1
  end
  array
end

my_each([1,2,3,4,5]) { |x| puts x * x }

# OUTPUT:
# 1
# 4
# 9
# 16
# 25

=> [1, 2, 3, 4, 5]
```

`collect` dissected:

```ruby
def my_collect(array)
  i = 0
  collect = []
  while i < array.length
    collect << (yield(array[i]))
    i += 1
  end
  collect
end

collect_results = my_collect([1,2,3,4,5]) { |x| x * x }

=> [1, 4, 9, 16, 25]
```

`select` dissected:

```ruby
def my_select(array)
  i = 0
  select = []
  while i < array.length
    if (yield(array[i]))
      select << array[i]
    end
    i+=1
  end
  select
end

select_results = my_select([1,2,3,4,5]) { |x| x == 3 }

=> [3]
```

BONUS `none?` dissected:

```ruby
def my_none?(array)
  i = 0
  while i < array.length
    if (yield(array[i]))
      return false
    end
    i += 1
  end
  true
end

my_none?([1,2,3]) { |x| x == 3 }

=> false

my_none?([1,2,3]) { |x| x == 4 }

=> true
```

BONUS `include?` dissected:

```ruby
def my_include?(array)
  i = 0
  while i < array.length
    if (yield(array[i]))
      return true
    end
    i += 1
  end
  false
end

my_include?([1,2,3]) { |x| x == 3 }
=> true
my_include?([1,2,3]) { |x| x == 4 }
=> false
```

These examples are simple and isolated, but `yield` truly provides myriad possibilities when building sophisticated programs. To prove it, check out this <a href="http://rubylearning.com/blog/2010/11/30/how-do-i-build-dsls-with-yield-and-instance_eval/" target="_blank">blog post</a> by Michael Bleigh where he described constructing eloquent looking code to design a DSL (Domain Specific Language) with the help of `yield`.

Have you ever used `yield` to do something clever or noteworthy? Sound off in the comments!

