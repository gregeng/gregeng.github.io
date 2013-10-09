---
layout: post
title: "Dissecting Common Ruby Iterators With Yield"
date: 2013-10-08 08:38
comments: true
categories: ruby
published: false
---


To understand the abstract though, we first need to understand the details. So lets take a look:

When you break it down, the ruby `each` method is just an abstraction of the following:

http://www.tutorialspoint.com/ruby/ruby_blocks.htm

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

=> [1, 2, 3, 4, 5]
```

Here it is dissected:
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

The select dissection:

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