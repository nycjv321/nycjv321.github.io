---
layout: post
title: What is the Testing Pyramid?
subtitle: The Testing Pyramid Series (Part 1)
tags:
  - testing
  - testing pyramid
  - automation
comments: false
published: true
date: '2020-01-07'
---

## The Testing Pyramid Series
_What exactly is the testing pyramid? What is the difference between an integration test and a system test? Are system tests integration tests? When should I be writing unit tests and when should I be writing integration tests? Can I write too many tests? Are all tests useful? Why even test at all?_ 

### About the Series

This is a first part of a multi-part series where were explore the traditional testing pyramid, challenge it, and try and answer common questions about testing. 

## What is the Testing Pyramid?

The testing pyramid is a concept originally described by Mike Cohn in his book _Succeeding with Agile_. Today there are many forms of the testing pyramid. The traditional pyramid consists of three tiers: UI (at the top), Service/API (in the middle) and Unit (at the bottom). The idea being that the majority of your tests are unit tests and your UI test suites being the smallest. 

There have been other versions of the testing pyramid. For our purporses, let's consider these three forms of testing: 
1. unit
1. integration
1. system 

We don't have to think of these types as of testing in the form of a traditional pyramid - maybe something more configurable and that you can tailor to your needs. 

That being said, I think we can all agree that we need less system tests. They are the slowest, least reliable, and most expensive. More on that later. 

Regardless, I still call it the testing pyramid - but maybe with air-quotes. But if you want... we can call it something else. How about the _Testing Stack_ or _Testing Pyramid-like_? Meh... Let's stick with testing pyramid for now. And for the record: sometimes more unit tests aren't a good thing. More on that later.

#### Unit Testing
Unit tests are tests that test individual functions. 

_test x ↦ f (x) produces y_

##### Pros

Unit tests are great for many reasons:

1. fast
1. deterministic
1. consistent 
1. cheap

##### Cons
But if you've ever written a lot of unit tests you may have run into some of the problems listed below:

1. coupling of tests with implementation (resulting in a lot of refactoring as requirements change)
1. hard to test from the user perspective
1. hard to test complicated workflows
1. provides fake confidence in functionality (_I wrote a bunch of unit tests!?! ... but they didn't really test the thing you did in the database_)

I've found that unit tests are great in certain situtations. When you want to test individual functions and algorithms. They let you quickly answer questions such as does `my_cool_function(foo: any)` actually return `bar: Thing`, or is my algorithm working? It doesn't make much sense to write a complete system test for this. But at the same time it depends on what `my_cool_function` is doing and where it's being used in your application's stack. This leads me to my next, and favorite, form of testing.

#### Integration Testing

Integration testing is the act of testing multiple units of code at once. _What does this really mean?_ Well it depends. It really is up to you. I would argue anything that isn't a unit test can be considered a form of integration testing. Modern software is complicated and consists of countless number of nested and interrelated functions. So it becomes a point of further classification, for example: api testing, greybox testing, system testings. Some examples of integration tests that come to mind:

* testing a function that depends on many more functions
* end-to-end testing (those are a lot of functions 😄)
* testing your backend's internal _service_ layer mocking out a database (or not) 

This ambigiouty is a double edged sword. Ultimately, it's up to you the software practitioner to define what they mean to you and your team. 

I tend to treat integration tests as slimmed down system tests where my tests functiotn as codified contracts at api boundries. I design my test's "plumbing" in such a way where I swap out unreliable and non-determistic components with ones that adhere to those component's contract's and are test-friendly implementations that allow my teammates to more efficienly test functionality and workflows without having to always test the non-deterministic and complex nature of the systems. Those unreliable and non-deterministic components are tested in isolation and their _interesting_ behaviors is understood and documented. But we don't allow their _interesting_ behavior to adjust our approach to testing the rest of our apps.

I've noticed that this sentiment isn't shared by a lot of folks. But I've found if you start breaking down your system into smaller digestible chunks and the interactions between them as codifiable contracts and test those contracts - testing and developing becomes much easier, faster, and maybe a little more fun.

##### Pros
1. Allows for expressing complex user scenarios
1. Much faster then system tests
1. Can mix-and-match mocks for testing complex workflows that involve external systems (more on that later in this series)
1. Allows you to do black box, white box, or grey box testing depending on your needs
1. Allows you to test from the user's perspective
1. Biggest bang for your buck

##### Cons
1. Integration tests vary between systems, tech stacks, and companies. Some companies write integration tests that mock out large part of systemts largely invalidating the whole point of testing
1. Can be slow if written inefficiently (e.g. reprovisioning systems for each test)
1. coupling of tests with implementation

#### System Tests

These are your slowest form but most thorough form of tests. System tests are a black box or a grey box form of testing when you interface with a running and fully provisioned system. This is great since it tests everything. But at the same time it's not ideal since you are testing everything at the same time everytime. They are also non-determinist in nature. Things fail in environments that don't get the same TLC as production and not understood by stakeholders other than those that provisioned them. 

_Why does email not work? Is it broken?!? ... No we just didn't configure it..._

I've seen great amount of emphasis on system tests when many verifications can be placed lower down the stack as integration tests or unit tests. Before you write that (unnecessarily) complicated system test (e.g. a UI test or testing a RESTful api against an external service) ask yourself or the developer writing the featutre if they wrote tests for the feature and what kind of tests. These sort of converations will be covered in later articles.

##### Pros
1. Allows for expressing complex user scenarios
1. Allows you to test as the end user
1. Thorough
1. Allows you to do testing with external systems 

##### Cons
1. Slow
1. Nondeterminstic
1. Hard to maintain
1. Expensive
1. Tests external systems

Later articles in this series will go into this concept - focusing more on unit and integration testing and less system testing.
