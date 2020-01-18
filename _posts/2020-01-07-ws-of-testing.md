---
layout: post
title: The Many Ws of Testing
subtitle: Who, What, When, Where, Why
tags:
  - five ws
  - testing
  - automation
comments: false
published: true
date: '2020-01-09'
---

## Background

Often when inheriting code the first thing I check for is test coverage. This may be a strange place to look. But I've found that you very quickly understand the quality of the codebase by inspecting the tests. 

_Things randomly break in production_ -> _No Tests_. 

_What if_ there were tests? _What if_ we went back and added tests for the bugs we found or keep finding in production? Would those things break again? What does this do for you and the team? Maybe less time maintaining and fighting 🔥s and more time working on new features and other value-adding activities.

Well-written tests help developers drive ~~architecture~~ design choices of their systems or reasonably justify them 😄. Tests also provide future contributors a sense of security that their changes didn't break an existing feature. 

I tend to appreciate well-designed systems. I also appreciate being able to understand the changes needed and how my changes affect the system. Having these things provide me a sense of comfort. I want to feel comfortable making changes. Also I don't like 🐛s. 🐛s annoy customers. Annoyed customers potentially costs your organization money.

## The Many Ws of Testing

I'm going to assume you are a naturally curious person and question everything. Hopefully, you ask questions about your systems and its tests. I'm probobly not as curious as you. But sometimes when considering tests I always try to think about the 5 ws Who, What, When, Where, Why. And no not only 5 questions:

* Why do we need to write tests? (The most important question is why!)
* Who are my tests about (the user persona)?
* Who is going to maintain the tests? (Me? Developers? Quality Engineers? No one?)
* What is being tested?
* What type of tests are needed? (What are the requirements -> unit test or integration test? or both?!?)?
* When are the tests going to be ran (during CI, on schedule, or never)?
* When to not write tests?
* Where do the tests run?  (On my machine? on your machine? On Jenkins?)?
 
## Who, What
You may be thinking that these some of questions mirror questions you would ask as a part of requirements gathering. Good point! You should already have the answers to some of these: who & what. (who) User Xn wants (what) feature A to adhere to the following specifications Ax (the whats). 

This drives the answers to other questions: Do the specifications result in code that adds behavior expressed through an API? Are you implemeting a language recognition system? Or perhaps you are implementing some obsecure algorithm from scratch. It may not make sense to write API tests for an algorithm. Unit tests would suffice to know if the algorithtm "works". But how do you know it works for the API? And how do you know that it, the API, still works when you make changes to it? Write some API tests! 

## Where and When
Sometimes you find yourself asking or being asked: _Where are these tests going to run?_ Hopefully, this is a question you are being asked when your team is ramping up. You may be suprised that sometimes organizations have tests that still don't run as part of continous integration processes.

### Continous Integration
If your working at an _Agile_ organization, your services _should_ have comprehensive integration tests and unit tests that run on a build server (e.g. Gitlab or Old Man Jenkins). The build process may include other things, for example: vulnerability scanning or code coverage. For the purposes of this article, we won't talk about that. So assuming you have some sort of automated tests: perhaps you have staged tests: unit tests & relatively quick integration tests on feature branches and more comprehensive integration tests on merge to develop? What's important is that the tests provide continous feedback as a part of their continous integration: _whoops my code broke some stuff I didn't consider_ .

_I didn't run the entire test suite locally... don't tell anybody about it!_

This feedback loop happens on a build server. Not on your machine or on the machine of your really smart coworker that everyone is gushing about all the time. That being said, sometimes it worth asking when are our tests running and which tests? Can we depend on our unit tests? Do the system tests provide a process that faciliates continous integration? Continous Integration provides the following benefits:

* Centralized Reporting and Notications (_Javier broke the build again!_)
* An automated feedback loop. Have you ever been asked to run a test suite that hasn't been ran in a while? Not fun. You should aim to ensure this feedback loop is 1) valuable and 2) quick. You lose a lot of value in finding a regression a week after it's been merged vs learning about it as soon as you push your code. 
* Dedicating machine to offload usage of system resources 

## Costs of Skipping on Tests
Many organizations rely on comprehensive system regression to answer very basic functional questions. However, system tests are slow and unreliable. And sometimes these tests are written after the fact. If you choose to rely on system regression you end up sacrificing relatively quick feedback for hours of execution, debugging, and maintenance. 

From a business point of view, you can view this as an opporuntity cost. It's easy to implement a feature that provides revenue (opportunity) and skip out on tests (cost). But did you factor in the cost that the bugs bring either as a part of the release or a subsequent release - because you rushed your software out the door?  Do you factor in the cost of losing customers because of your buggy software? Or the increased maintainance costs? This feels like a lot of cost and lost revenue for a small opportunity.

_Oh we care about tests_

What type of tests do you care about? You may find yourself in interviews asking how much developers, and future coworkers, care about tests. It may be more productive, to instead, ask what type of tests do your colleagues care about? And do they really care about them?  Or more importantly do they understand the value that tests bring to the organization? And what does testing mean to your stakeholders? Alternatively, are they looking to grow their testing culture or perhaps someone to lead on growing the testing culture?

But do you want to deal with the _unknown costs_? Tests constantly failing and tests that require constant updates? This isn't comforting and is very expensive. People start to trust the tests less  and less, and eventualy start ignoring them. And then it happens ... A nasty 🐛 gets into production. Right around that time, that overworked QA/Test Automation Engineer finally gets down to that one service with all those flaky tests and finds an automated test expressing that very same 🐛. People start asking: Why didn't anyone notice this? Why did our QA person miss this?!? 

Some bugs are inevitable. Sometimes when scaling, systems act differently and unexpected. That is a really hard problem to solve. 

_💩 happens!_

But what's important is addressing those issues when they happen and retrospecting on them and preventing similiar issues going forward. In my experience, other bugs related to application functionality are due to: 

* Poor test coverage
* Vague requirements that force developers to make weird design decisions resulting in unexpected/unintentional behavior
* Poor Systems Design/Cohesiveness - which is something that can be seen as either a precursor or a consequence of the previous points

## Who (is going to maintain the tests)

This is a very interesting question. I've seen most organizations default to non-development stakeholders who will by default write system tests - ui tests or traditional api tests. This may be news to many folks. But developers can write automated tests as well. 🤯 Unit tests and Integration tests are a form of automated testing 🤯. [Crazy](https://giphy.com/explore/mind-blown) right? Check out [this](https://nycjv321.github.io/2020-01-07-testing-pyramid-services-pt1/) article on the testing pyramid that breaks down the differences. 

That being said it may be easier to write system tests. And interestingly enough record and playback tools are making a resurgence. But what about maintaining them? The answer to who maintains your tests also influences the answers to your other questions: Where are these tests ran? How often are they ran? If you let QA engineers run your tests - they might or might not run them on your build server. If you let non-development staff run your tests based on the schedule they choose, developers might not know about regressions until it's too late into the sprint. Why not let your developers own the automation efforts and empower them to automate the trivial things? 

_but developers don't understand automation_ 

I've read many very unconvincing articles that developers shouldn't be trusted to own the automation efforts. This will be a topic for another article. 


## Why

A few questions to ask:
Why do we need tests?
Why does the PR not include tests?
Why did the original feature not include tests? Did we rush it to satisfy some customer request? _Yes..._ That's the realities of business. Did the original author not value tests? 

It's good to be asking _Why_. But are we asking _why_ too late? 

_It is what it is_ 

But do you value tested and testable software? Does your team? Does your organization?

_Why is this broken?_ Maybe not the most productive question to ask.

Did you ask why didn't the code review that submitted that change contain tests? Did those tests provide adequate coverage in expressing the implemented requirements? Did you ask why the original feature that that change built on-top of not have tests? Did we answer when it was acceptable to not write tests? Maybe not a why: but I want to ask this: what is the opportunity cost of not writing tests? 

These are questions we should be asking. 

The answers to these questions can be part of our _definition of done_. I've known teams that have agreed to having automated functional tests as a part of their code reviews 😎. If this article has been even remotely engaging - hopefully it's done a decent job of providing the breadcrumbs to the answer to the question: _Why do we need to test?_. 

In another article I will try and answer the question of how to test.
