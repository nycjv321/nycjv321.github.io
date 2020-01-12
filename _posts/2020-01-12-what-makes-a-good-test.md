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

First, let's step back and define a test. One definition that I found, quiet suiting for us as software practioners, from [Merriam-Webster](https://www.merriam-webster.com/dictionary/test) is 

_the procedure of submitting a statement to such conditions or operations as will lead to its proof or disproof or to its acceptance or rejection_.

So let's think in broad terms. And let's define attributes and characteristics that define _good_ tests and _bad_ tests. In an Agile organization, focus is point on automation and utilizing it for various things including tests. For the purposes of this article we are talking about automated testing unless otherwise stated.

## What makes a good test?

So what exactly makes a good test? Instead of discussing "good" tests, let us instead talk about the bad things that can make or break a test. This will incentivise us to think about _ideal_ tests.

When I think about _bad_ tests this is what comes to mind:

* Low value-add
* Slow
* High Maintenance
* Expensive
* Lack of Team Ownership
* Non-determinstic/Flaky - randomly fail

Let's try and explain what exactly this means and some of the ways of addressing these characteristics.

### Low value-add

How do we define _low value-add_? From a business point of view I define low-value tests as tests that don't actually test the thing you are testing or does it in a way that isn't reflective of the customer expecation. 

Arguablly, all the things in the above section would make a test low value. But let's consider the actual intent of the test, its implementation, what's it's trying to do. There are a few things that always stick out to me. These include (but are not limited to):

* Not actually invoking the code that your are testing or claim to be testing
* Relying on Mocks
* Very poor defined assertions (`assertEquals(myThing.size(), 1)` or `assertNotNull(myThing)`);

Does this ring a 🔔? 

Tests and the act of testing should be high value-adding activity. Otherwise why waste your time doing them? Tests should enable you and your team to know that your code works as advertised. Now there are different ways to incoporate tests into your SDLC.

One measure of a scrum team's productivity is it's velocity. And one of the things that affects velocity is handling the unknowns that come up during a sprint. This would include bugs found by your tests and production bugs. One of the things that should be a discussion point for your retrospectives (or Kaizen's if you practice Kanban) is: How do we limit these unknowns and regressions found by QA or potentially as 🐛s by your customers? Handling unknowns is a topic for another day. We can limit regressions if we have test suites that we can _depend on_ and test suites that run as a part of feedback loop that matches the needs of your organization's velocity. 

_depend on_

Can we depend on our tests? Should we depend on our tests?

Well have you haver seen a test like this?

{% highlight java linenos %}
public void test() {
    MyClass dependency = mock[MyClass]
    when(dependency.doesStuff()).thenReturn("stuff");
    TestClass testClass = new TestClass(dependency)
    assertEquals(testClass.doesStuff(), "stuff");
}
{% endhighlight %}

Can we depend on this code to tell us how `MyClass` functions? No. This is because we mocked out `MyClass`. This is a very simple example at a very micro-view of our potential code. How does this look at the macro level? Does your codebase have tests for all of your `MyClass`es where it makes sense? 

What are the value-add of your tests? Can you say that you currently `depend on` your tests?

In order to be able to depend on tests they need to be high value add. _How do we do this?_ T

Some of the following talking points contribute to idea of understanding the value-add proposition of your tests. If you have flaky, slow, and high haintenance tests, they might not be high value add even if they test all if the `MyClass`es in your codebase and provide extensive regression coverage. 

### Slow

`Slow` is a relative term. It depends on the team and the organization. For some folks, a unit test that takes 100ms might be considered slow while a system test that takes 100ms is considered incredible fast. But a test of any form that takes 5 minutes is considered slow vs for example 2 minutes. What about 500 integration tests that take 5 minutes to run (0.6 seconds per test) vs 500 system tests that each take 2 minutes toe execute? How fast (or how slow) is that test suite?  

How do you define a slow test? How does your team or organization define slow? Is slow acceptable? When is slow unacceptable? Is slow acceptable when we are debugging that test locally? Or is unacceptable when monitoring the build server? Can we make this test quicker? .... 

_What is this slow test actually even doing?_

In my experience most _slow_ tests are system tests or integration tests that either require optimization or are unnecassairly testing multiple things too late in the software lifecycle. In terms of express complex requirements, integration tests should your go to. Integration tests are also easier to optimize becuase we can more control over what part of the app we test and how we initialize our context. For our slow tests can we move them further down the stack? Can we move our testing from system tests that take 2 or even 5 minutes to execute down to our integration tests that only take .06 seconds to execute? Or even further down to our unit tests? Does it make sense to test it at a unit level without doing some crazy mocking? Is crazy mocking worth it?  More on this in another article.

### Flaky

What are flaky tests? They are tests that randomly fail. Have you wrote a test and it passes locally. But it fails on the build server immediately? Or on the build server a weeks later? But it passes locally? Or it fails remotely on the server but only occasionally or right before a release cut-off window? 

How do we make our test less flaky? I'll go into more detail on that in another article but here are a few tips:

#### UI Tests
* Rely on generic css selectors (classes vs auto-generated xpaths)
* Use [Page Objects or Page Components](https://martinfowler.com/bliki/PageObject.html) liberally
#### API Tests
* Don't hard code message assertions unless you are testing in the same code base as the messages
#### General Practices
* Be [Dry isntead of Wet](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)
* Don't rely on hard-coded things to do your assertions. 
* Create data on the fly so you can test that directly vs stale data
* Don't rely on seeded data
* Elimite unnecassary entropy from your tests when not the point of your tests
* Ensure your build server and test environments are optimized to actually run your tests
* Swap out flaky dependencies with test-friendly implementations and test flaky dependencies in isolation or have their idiosyncrasies documented and understood.
* Don't rely on system testing when you can write unit or interation tests.

### Lack of Team Ownership

This is point that I don't think gets discussed enough. A question to ask is: who owns your tests? Do you own the tests? Does your super smart team member own the tests? Does QA own the tests? Do the developers own the tests? Most importantly... 

_Does the team own the tests?_

In many organization's I've worked with I've seen that test owership is either poorly defined or that tests are owned by particular individuals or isolated teams. While it's good that someone wants ownership of your testing efforts. I always wonder if the ownership is actually counterproductive to the team's efforts? Instead of understanding your testing efforts, does your team depend on one person or a team for test results? How does this affect your feedback loop? Is your team waiting weeks, waiting a week, or a few days? Or is your test feedback immediate? Are QA resources free to test other things? Work on new tests? Free to do exploratory testing? Or are they heads down "writing tests"? Did your developers write tests Is QA and the development team duplicating testing efforts? 

_What if_ the team collectivelly owned your tests? And your developers collaborated with QA in the automated testing efforts? What if they broke down where they tested and what was tested?  What if feedback was immediate and a part of your build process? 

What is the value proposition in your current process? What value position would your organization value?

### High Maintenance

Not going to talk about this much. But if you write a test and you find yourself having to revisit it outside of changing requriements perhaps there is a problem. Maintenance is tied to Flakiness. The less flaky your tests are the less maintenance you will have to do on them. That being said, flakiness isn't the only component of high or unnecassary maintenance. Did a field change and now you have to update 30 tests? Are you abstracting your [APIs](https://en.wikipedia.org/wiki/Application_programming_interface) interactions in such a way that changes are minimal and related to functional requirements vs having to update tests because are having to do refactoring? Are you following [SOLID principals](https://en.wikipedia.org/wiki/SOLID)?

#### A note about refactoring

Good tests should help you drive your development process. Perhaps you practive TDD or some variation of it like ATDD or BDD. Refactoring during test creation is expected and abnormal otherwise. Tests allow you to question design decisions and requirements and reaffirm them. It may take a few iterations, but utilizing testing practices will always result in higher quality code. That being said if you are having to constantly refactor tests with multiple requirements perhaps your missing an abstraction to simplify complexity. 

### Expensive

Personally, I've always found that it's hard to quantify the costs of your tests. I think there are a lot of things to consider when it comes to costs and ROI:

* Costs to Implement
* Costs to Run
* Cost to Maintain
* Costs of regressions
* Costs of reoccuring regressions

Are all these costs required? Are these all costs? You should be probobly be looking at some of these as investments first and then consider the costs. Everything has a cost. But not everything needs an investment. It is possible to invest too much into your testing. 

Does your team view implementing testing an investment or a cost first? Is it both? In that list above do you see any costs  that can be avoided or reduced? Are these costs that your team currently have? Are these costs understood or even discussed by your team members? Are the costs of your testing strategy been something your team has agreed to? Are there costs that your team is working on reducing them (e.g. adding missing regression, reducing execution team)?


