---
layout: post
title: "Abstraction and 3 Helpful Ruby Iterators"
date: 2013-10-09 18:30
comments: true
categories: ruby
published: true
---
I've never been a big fan of modern art. Some pieces are so abstract that I don't really get the point - and I know I'm not the only <a href="http://www.buzzfeed.com/jenlewis/quiz-can-you-tell-the-difference-between-modern-art-and-art
" target="_blank">one</a>.

But in programming, abstraction is different. It makes for more eloquent and productive code by concealing complexity. Let's witness abstraction by stepping through 3 related methods: 
`each`,  `collect`  &  `select`.

The `each` method:

```ruby
numbers_array = [1,2,3,4,5,6,7,8,9,10]

    numbers_array.each do |x|
      print "#{x * 2} "
    end

#output: 2 4 6 8 10 12 14 16 18 20

numbers_array = [1,2,3,4,5,6,7,8,9,10]

```

The `each` method iterates through individual elements in the array and returns the original array untouched.

When I first learned the `each` method, I used it to almost exclusively when working with arrays to make the computer do alot of the leg work for me.

<a href="http://en.wikipedia.org/wiki/Abraham_Maslow" target="_blank">Abraham Maslow</a> explains this consequence:
>...if all you have is a hammer, everything looks like a nail.

However, since the `each` method does not change the original array, I started writing alot of code that looks like this:

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
Here, I am creating a new empty array, just so I can shovel items into it with the `each` method. If this feels bulky to you, that's because it is! Programmers might say this pattern reeks of *code smell*.

The ruby `collect` method is a better tool for the job, and it is just an abstraction of the `each` method.

Here is `collect` using the example from above:

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
Note that the `collect` method returns a **new** array which I have set equal to "odds_and_ends."

But what if we only wanted to return a *part* of the array if it matches certain criteria? In that case, `collect` no longer the best option.

As you might have guessed, the ruby `select` method is an abstraction of the `collect` method. It also returns a new array, with an implicit *if* clause built in.

Continuing with our example...

If we were to use `collect`, it would return a messy array including nil values...

```ruby
odds_and_ends = [1,2,3,4,5,6,7,8,9,10].collect do |x|
  if x % 2 != 0 
    x
  end
end

=> [1, nil, 3, nil, 5, nil, 7, nil, 9, nil]

```

...while `select` gets the job done more cleanly. It uses the implicit if to return only values matching the specified logic:

```ruby
odds_and_ends = [1,2,3,4,5,6,7,8,9,10].select do |x|
  x % 2 != 0
end

=> [1, 3, 5, 7, 9]
```

So remember, abstraction is your friend! 

To recap: `select` is an abstraction of `collect` which is just an abstraction of `each`. Use them to <a href="http://images.wikia.com/richmoreacademy/images/0/07/Pine-tree-car-air-freshener.gif" target="_blank">freshen</a> up any and all code smell. I'm still working to make sure I choose the right methods. 

By the way, if you were wondering where the `each` method comes from, you would be right to assume it is just another abstraction of simpler ruby properties! In a future blog post, I will go over how all of these iteration methods break down using the `yield` statement. 

