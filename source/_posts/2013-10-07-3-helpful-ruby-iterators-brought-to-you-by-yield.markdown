---
layout: post
title: "Abstraction and 3 Helpful Ruby Iterators"
date: 2013-10-07 23:10
comments: true
categories: 
published: false
---
I've never been a big fan of modern art. Some pieces are so abstract that I don't really get the point - and I know I'm not the only <a href="http://www.buzzfeed.com/jenlewis/quiz-can-you-tell-the-difference-between-modern-art-and-art
" target="_blank">one</a>.

But in programming abstraction is different. It makes for more eloquent and productive code by concealing complexity. Let's see abstraction occur by stepping through 3 related methods: 
`each`,  `collect`  &  `select`.

<!-- To understand the abstract though, we first need to understand the details. So lets take a look: -->

When you break it down, the ruby `each` method is just an abstraction of the following:

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

The `each` method returns the original array.

When I first learned the `each` method, I used it to almost exclusively to iterate over arrays. According to Abraham Maslow this is a consequence because "if all you have is a hammer, everything looks like a nail."

A common pattern that started occuring looked like this:

```ruby
numbers_array = [1,2,3,4,5,6,7,8,9,10]
odds_and_ends = []
    
    numbers_array.each do |x|

      if x % 2 != 0 
        odds_and_ends << true
      else x % 2 == 0 
        odds_and_ends << false
      end

    end

odds_and_ends 

=> [true, false, true, false, true, false, true, false, true, false]
```

If this feels bulky to you, that's because it is! Programmers might say this pattern reeks of *code smell*.


The ruby `collect` method is a better tool for the job, and it is just a further abstraction of the `each` method.

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

Here it is using the example from above:

```ruby
odds_and_ends = [1,2,3,4,5,6,7,8,9,10].collect do |x|
  if x % 2 != 0 
    true
  else x % 2 == 0 
    false
  end
end

=> [true, false, true, false, true, false, true, false, true, false]
```
It's important to note that the `collect` method returns a **new** array.

But what if we only wanted to return values matching certain criteria? Then `collect` no longer is the right tool for the job. 

As you might have guessed, the ruby `select` method is an abstraction of the `collect` method.

The dissection:

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

Continuing with our example...

If we were to use `collect`, it would return a messy array including nil values:

```ruby
odds_and_ends = [1,2,3,4,5,6,7,8,9,10].collect do |x|
  if x % 2 != 0 
    x
  end
end

=> [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]

```

`select` gets the job done more cleanly:

```ruby
odds_and_ends = [1,2,3,4,5,6,7,8,9,10].select do |x|
  x % 2 != 0
end

=> [1, 3, 5, 7, 9]
```

So remember, abstraction is your friend! To recap: `select` is an abstraction of `collect` which is an abstraction of `each`. Use them to <a href="http://images.wikia.com/richmoreacademy/images/0/07/Pine-tree-car-air-freshener.gif" target="_blank">freshen<a> up any and all code smell. 

By the way, if you were wondering where the `each` method comes from, you would be right to assume it is just another abstraction of simpler ruby properties! In a future blog post, I will go over how all of these iteration methods break down using the `yield` statement. 

