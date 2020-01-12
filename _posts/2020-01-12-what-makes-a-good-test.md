---
layout: post
title: What makes a good test?
subtitle: Identifiying smells in your tests
tags:
  - tests
comments: false
published: false
date: '2020-01-12'
---

## What do we mean by a "test"?

So this article title was pretty vague. What do we mean by _test_? Do we mean a manual test? Do we mean an automated ui test? Do we mean a unit test?

Good question!

Let's step back and define a test. One definition that I found quiet suiting for purposes from [Merriam-Webster](https://www.merriam-webster.com/dictionary/test) is 

_the procedure of submitting a statement to such conditions or operations as will lead to its proof or disproof or to its acceptance or rejection_.

So let's think in broad terms. And let's define attributes and charaecterisistics that define _good_ tests and _bad_ tests.

## What makes a good test?

So what exactly makes a good test? Let's first think about all the bad things that make a test. This will incentivise us to think about ideal tests.

When I think about _bad_ tests this is what comes to mind:

* Low value-add - they don't actually test anything
* Slow
* High Maintenance
* Expensive
* Lack of Team Ownership
* Non-determinstic/Flaky - randomly fail

### Low value-add

How do we define _low value-add_? From a business point of view I define low-value tests as tests that don't actually test the thing you are testing. Now I've seen a few variations of this.

* Not actually invoking the code you are adding
* Relying on Mocks
* Very poor defined assertions (`assertEquals(myThing.size(), 1)` or `assertNotNull(myThing)`);

Does this ring a 🔔? 

Tests should be value-add. Otherwise why waste your time doing them? Tests should enable you and your team to know that your code works as advertised. Now there are different ways to incoporate tests into your SDLC. Traditionally software development tests depended on extended test cycles that depended on droves on manual testers. 

In an agile world, the core of the agility is the ability for the team to meet it's velocity. And one of the things that affects velocity is handling the unknowns that come up. This would include bugs found by your tests and production bugs. How do we limit these unknowns and regressions found by QA or potentially as 🐛s by your customers? Handling unknowns is a topic for another day. But we can limit regressions if we have test suites that we can depend on and test suites that run as a part of feedback loop that matches the needs of your organization's velocity. 

_depend on_

Well have you haver seen a test like this?

{% highlight java linenos %}
public void test() {
    MyClass dependency = mock[MyClass]
    when(dependency.doesStuff()).thenReturn("stuff");
    TestClass testClass = new TestClass(dependency)
    assertEquals(testClass.doesStuff(), "stuff");
}
{% endhighlight %}

Can we depend on this code to tell us how `MyClass` functions? No. This is a very simple example at a very micro-view of our potential code. How does this look at the macro level? Does your codebase have tests for all of your `MyClass`es where it makes sense? 

What are the value-add of your tests? Can you say that you currently depend on your tests?

In order to depend on tests they need to be high value add. _How do we do this?_ That will be an article for another day. 

Arguably the following points contribute to the value-add proposition of your tests. If you have flaky, slow, and high Maintenance tests. They might not be high value add even if they test all if the `MyClass`es in your codebase and provide extensive regression coverage. 

### Slow

`Slow` is a relative term. It depends on the team and the organization. For some folks, a unit test that takes 100ms might be considered slow while a system test that takes 100ms is considered incredible fast. But a test of any form that takes 5 minutes is considered slow vs 2 minutes? What about 500 integration tests that take 5 minutes to run (0.6 seconds per test) vs 500 system tests that each take 2 minutes? How fast is that test suite? 

How do you define a slow test? How does your team or organization define slow? Is slow acceptable? When is slow unacceptable? Is slow acceptable when we are debugging that test locally? Or is unacceptable when monitoring the build server? Can we make this test quicker? .... 

_What is this slow test actually even doing?_

In my experience most _slow_ tests are system tests or integration tests that require optimization. Integration tests are easier to optimize becuase we can more control over what part of the app we test and how we initialize our context. For our slow tests can we move them further down the stack? Can we move our testing from our 2 minute system tests down to our .06 second integration tests? Or even further down to our unit tests? Does it make sense to test it at a unit level without doing some crazy mocking? Is crazy mocking worth it?  

### Flaky

What are flaky tests? They are tests that randomly fail. Have you wrote a test and it passes locally. But it fails on the build server immediately? Or on the build server a weeks later? But it passes locally? But it fails remotely on the server occasionally? 

How do we make our test less flaky? I'll go into more detail on that in another article but here are a few tips:

#### UI Tests
* More generic css selectors (classes vs auto-generated xpaths)
#### General Practices
* Don't rely on seeded data
* Don't rely on hard-coded collection sizes. 
* Create data on the fly so you can test that vs stale data
* Elimite unnecassary entropy from your tests when not the point of your tests
* Ensure your build server and test environments are optimized to actually run your tests

### Lack of Team Owernship

This is point that I don't think gets discussed enough. A question to ask is who owns your tests? Do you own the tests? Does your super smart team member own the tests? Does QA own the tests? Do the developers own the tests? Most importantly... does the team own the tests? 

In many organization's I've worked with I've seen that test owership is either poorly defined or that tests are owned by particular individuals or isolated teams. While it's good that someone wants ownership of your testing efforts. I always wonder if the ownership is actually counterproductive to the team's efforts? Instead of understanding your testing efforts, does your team depend on one person or a team for test results? How does this affect your feedback loop? Is your team waiting weeks, waiting a week, or a few days? Or is your test feedback immediate? Are QA resources free to test other things? Work on new tests? Free to do exploratory testing? Or are they heads down "writing tests"? Did your developers write tests Is QA and the development team duplicating testing efforts? 

_What if_ the team collectivelly owned your tests? And your developers collaborated with QA in the automated testing efforts? What if they broke down where they tested and what was tested?  What if feedback was immediate and a part of your build process? 

What is the value proposition in your current process? What value position would your organization value?

### High Maintenance

Not going to talk about this much. But if you write a test and you find yourself having to revisit it outside of changing requriements perhaps there is a problem. Maintinance is tied to Flakiness. The less flaky your tests are the less maintenance you will have to do on them. Flakiness isn't the only component of maintenance. Did a field change and now you have to update 30 tests? Are you abstracting your APIs interactions in such a way that changes are minimal and related to functional requirements vs having to update tests because are having to do refactoring?

#### A note about refactoring

Good tests should help you drive your development process. Perhaps you practive TDD or some variation of it like ATDD or BDD. Refactoring during test creation is expected and abnormal otherwise. Tests allow you to question design decisions and requirements. It may take a few iterations, but tests will always result in higher quality code. That being said if you are having to constantly refactor tests with multiple requirements perhaps your missing an abstraction to simplify complexity. 

### Expensive

Personally, I've always found that it's hard to quantify the costs of your tests. I think there are a lot of things to consider when it comes to costs and ROI:

* Costs to Implement
* Costs to Run
* Cost to Maintain
* Costs of regressions
* Costs of reoccuring regressions

Are all these costs required? Are these all costs? You should be probobly be looking at some of these as investments first and then consider the costs. Everything has a cost. But not everything needs an investment. It is possible to invest too much into your testing. 

Does your team view implementing testing an investment or a cost first? Is it both? In that list above do you see any costs  that can be avoided or reduced? Are these costs that your team currently have? Are these costs understood or even discussed by your team members? Are the costs of your testing strategy been something your team has agreed to? Are there costs that your team is working on reducing them (e.g. adding missing regression, reducing execution team)?


