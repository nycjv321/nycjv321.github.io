---
layout: post
title: 5 Ws of Testing
subtitle: Who, What, When, Where, Why
tags:
  - five ws
  - testing
  - automation
comments: false
published: false
date: '2020-01-09'
---

## Background

Often when inheriting code, as I often tend to do, the first I check for is test coverage. This may be a strange place to look. But I've found that you will find out really quickly determine the quality of the code base by inspecting the tests. _Things randomly break in production_ -> _No Tests_. What if there were tests? What if we went back and added tests for bugs? Do those things break again? 

Well-written tests help developers drive architecture choices of their systems or reasonably justify them :). Tests also provide future contributors a sense of security that their changes didn't break an existing feature. 

I tend to appreciate well designed systems and being able to accurately describe how my changes affect the system. Having these things provide me a sense of comfort. I want to feel comfortable making changes. Also I don't like 🐛s. 🐛s annoy customers and cost money.   


## The Many Ws of Testing

I'm going to assume you are a naturally curious person and question everything. Hopefully you ask questions about your systems and their tests. I probobly not as curious as you. But sometimes when considering tests I always try to think about the 5 ws Who, What, When, Where, Why. And no not only 5 questions:

* Why do we need to write tests? (The most important question is why!)
* Who am are my tests about (the user persona)?
* Who is going to maintain the tests? (Me? Developers? Quality Engineers? No one?)
* What is being tested?
* What type of tests are needed? (What are the requirements -> unit test or integration test? or both?!?)?
* When are the tests going to be ran (during CI, on schedule, or never)?
* When to not write tests?
* Where do the tests run?  (On my machine? on your machine? On Jenkins?)?
 
## Who, What
You may be thinking that these some of questions mirror questions you would ask as a part of requirements gathering. Good point! You should already have the answers to some of these: who & what. (who) User X, Y, and Z want (what) Feature A to adhere to the following specifications A, B, & C. That's the what. 

This drives the answers to other questions: In order to implement the specifications that the feature is about you may need some API tests? Are you testing some complex parsing function? Are you testing some natural language processing feature? Or perhaps you are implementing an algorithm from scratch. It may not make sense to write api tests for an algorithm. Unit tests would suffice to know if the algorithtm "works" But how do you know it works for the api? Write some API tests! 

## Where and When
Sometimes you find yourself asking or being asked: _Where are these tests going to run?_ Hopefully this is a question you are being asked when you are ramping up. You may be suprised that sometimes tests still don't run during continous integration processes.

### Continous Integration
If your working at an _Agile_ organization, your services should have comprehensive integrations and unit tests that run as a part on some build server (e.g. Gitlab or Old Man Jenkins). The build process may include other things, for example: vulnerability scanning or code coverage. For the purposes of this article we won't really talk about that. So assuming you have some sort of automated tests: perhaps you have staged tests: unit tests & relativly quick integration tests on feature branches and more comphresnive integration tests on merge to develop? What's important is that the tests provide continous feedback as a part of their continous integration: _whoops my code break some stuff I didn't consider_ .

_I didn't run the entire test suite locally... don't tell anybody about!_

This feedback loop happens on a build server. Not on your machine or the machine of your really smart coworker that everyone is gushing about all the time. That being said, sometimes it worth asking when are our tests running and which tests? Continous Integration provides the following benefits:

* Centralized Reporting and Notications (Javier broke the build again!)
* An automated feedback loop. Have you ever been asked to run a test suite that hasn't been ran in a while? Not fun. You should aim to ensure this feedback loop is 1) valuable and 2) quick. You lose a lot of value in finding a regression a week after it's been merged vs learning about it once you push your code. 
* Dedicating machine to offload computation 

## Costs of Skipping on Tests
Many organizations rely on comprehensive system regression to answer very basic functional questions. System tests are slow and unreliable. And sometimes these tests are written after the fact. If you choose to rely on system regression you end up sacrificing relatively quick feedback for hours of execution, debugging, and maintainance. 

From a business point of view, you can view this as an opporuntity cost. It's easy to imeplement a feature (opportunity) and skip out on tests. But did you factor in the cost that the bugs bring either as a part of the release or subsequent release? Do you factor in the cost of losing customers because of your buggy software? Or the increased maintainance costs? This feels like a lot of cost for a small opportunity.

_Oh we care about tests_

What type of tests do you care about? One questions I've asked in interviews is how much developers care about tests. A question I haven't asked enough is what type type of tests do your developers care about? And do they really care about them? Or more importantly do they understand the value that tests bring? And testing actually means when to your stakeholders?

But do you want to deal with the _unknown costs_? Tests constantly failing and tests that require constant updates? This isn't comforting and is very expensive. People start to trust the tests less and start ignoring them. And then it happens. A nasty 🐛 gets into production. Right around that time, that overworked QA/Test Automation Engineer finally gets down to that one service with all those flaky tests and finds a bug as well 🐛. People start asking: Why didn't anyone notice this? Why did our QA person miss this?!? 

## Why

A few questions to ask:
Why do we need tests?
Why does the PR not include tests?
Why did the original feature not include tests? Did we rush it to satisfy some customer request? _Yes..._ That's the realities of business. Did the original author not value tests? _It is what it is_ But do you value tests? Does your team and organization value automated tests?

It's good to be asking _Why_. But are we asking _why_ too late? 

_Why is this broken?_ Maybe not the most productive question to ask.

Did you ask why didn't the code review that submitted that change contain tests? Did you ask why the original feature that that change built on top of didn't have tests? Did we answer when it was acceptable to not write tests? Maybe not a why: but what is the opportunity cost of not writing tests? 

These are questions we should be asking. The answers to these questions can be part of our _definition of done_. I've known teams to have agreed to having tests as a part of their code reviews. If this article has been remotely engaging, hopefully it's done a decent job of providing the breadcrumbs to the answer to the question: _Why do we need to tests?_.