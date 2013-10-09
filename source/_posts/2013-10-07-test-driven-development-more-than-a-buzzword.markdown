---
layout: post
title: "Test-Driven Development:
More Than A Buzzword"
date: 2013-10-07 21:44
comments: true
categories: TDD
published: true
---

Before starting programming classes, I had heard of test-driven development (TDD). I was told it was a good practice, similar to the way I was told to eat my green vegetables when I was younger. That's why up until today, I thought it was just another business <a href="http://unsuck-it.com/high-level-overview/" target="_blank">buzzword</a>.

Today we began to cover the basic principles of object-oriented programming (OOP). Even in a simple app with 3 related classes, you can begin to see the complexity unfold. With complexity comes fragility ... and that’s where TDD comes into play.

Jason Arhart does a good job explaining TDD in his <a href="https://speakerdeck.com/lvrug/introduction-to-tdd-jason-arhart" target="_blank">speakerdeck</a>. Essentially, the process entails writing a simple test, letting it fail, and then writing the minimum amount of code to make the spec pass. He deftly sum's it up in a cyclical diagram describing the "Red", "Green", "Refactor" pattern. By following this method, you get a solid baseline of tests coupled with functioning code. 

It's this constant cycle that allows programs and their programmers to evolve. To this end, I think its better to think of it as an upward spiral. With this approach, the process may be a bit slower, but ultimately the sky is the limit. 

Jason describes TDD as a process that, "turns the 'traditional' on its head." I consider myself lucky to start learning in an environment where TDD is the convention. It feels great to pass each spec in a test suite and incrementally move from red to green. I liken it to the feeling you get when gambling and winning, only here you get to bet *with* the house.

If you want to learn more about TDD, Jason has another speakerdeck that describes the Ruby specific testing framework:<a href="https://speakerdeck.com/lvrug/rspec-for-beginners-jason-arhart" target="_blank"> RSpec</a>. We will be using more of RSpec as the semester continues at the Flatiron school.
