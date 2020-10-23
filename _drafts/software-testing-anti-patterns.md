---
tags:  testing wikimedia
title: Software Testing Anti-patterns
---
My team at work has a reading club. In October 2019 we've read [Software Testing Anti-patterns](http://blog.codepipes.com/testing/software-testing-antipatterns.html) blog post. I've liked the article. Maybe even more, I've liked the discussion we had about it. I would highly recommend reading the article.

That's just the beginning of the story. I've noticed that I was referring to the article frequently in conversations at work, conferences or meetups. It nicely sums up many of my thoughts. I've caught myself in several occasions saying something like "That's anti-pattern #7!"

This post spend a lot of time in draft mode. I wanted to write something but I didn't want to copy/paste the entire original article. I've ended up with quoting some parts of the article that I found particularly interesting and adding my comments here and there.

# A surprise

One of the things that keeps surprising me is how people that have a lot of experience with software development frequently have almost no experience with software testing.

# Terminology

I would like to discuss terminology really quickly. The problem is, if you ask three people about the difference between unit, integration and system tests, you'll probably get four responses. Sometimes even five.

So, let's define just a few basic terms, before we start the discussion.

> [Software testing](https://en.wikipedia.org/wiki/Software_testing) is an investigation conducted to provide stakeholders with information about the quality of the software product or service under test.

Software testing is investigation.

> In software engineering, a [software design pattern](https://en.wikipedia.org/wiki/Software_design_pattern) is a general, reusable solution to a commonly occurring problem within a given context in software design.

That's pretty clear.

> An [anti-pattern](https://en.wikipedia.org/wiki/Anti-pattern) is a common response to a recurring problem that is usually ineffective and risks being highly counterproductive.

Anti-pattern is a solution that creates more problems than it solves.

This is not a definition, but I would still like to mention it. Testing pyramid. (It's really a triangle, but let's skip that discussion for now.) I really like the image from the top of [TestPyramid](https://martinfowler.com/bliki/TestPyramid.html) article at Martin Fowler's blog. The following image is the only one I could find under an open license.

<a title="Abbe98, CC BY-SA 4.0 &lt;https://creativecommons.org/licenses/by-sa/4.0&gt;, via Wikimedia Commons" href="https://commons.wikimedia.org/wiki/File:Testing_Pyramid.svg"><img width="512" alt="Testing Pyramid" src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Testing_Pyramid.svg/512px-Testing_Pyramid.svg.png"></a>

This article will cover the bottom two parts of the pyramid (unit and integration). The top part (E2E) is a topic for another post.

# Events

I talked about software testing anti-patterns at [Zagreb Tech Sauna meetup](/zagreb-tech-sauna) in December 2019. I went over all thirteen anti-patterns from the blog post. With the introduction and some discussion, the talk ended up being over an hour long. A bit too long.

I gave a half hour talk at [Testival #56](/testival-56) meetup in January 2020, covering five anti patterns. I've heard comments that it was a bit too short.

I've started writing this blog post while preparing for Testival meetup. I've worked on it while preparing for [Quality and Test Engineering Office Hour](https://lists.wikimedia.org/pipermail/wikitech-l/2020-February/093084.html) in February 2020.

I've started writing this blog post while preparing for Testival meetup in January 2020. I've worked on it while preparing for Quality and Test Engineering Office Hour in February 2020. I've finished it in October 2020.

# Anti-patterns

In the talks, for each anti-pattern I've mentioned some advantages and disadvantages. It would be great to hear from the readers if they've seen them in the wild.

In this article, I'll usually quote the most important part of the original article and comment on it.

# Chords

To make it more fun, I've played a dissonant chord on a ukulele for every pattern. For my reference, the chords I've used are: Am6, A#6, Bm9, C7sus4, C#aug, Dsus4, D#9, Edim, Fsus2, F#m9, G6, G#aug. (I've spent some time searching for perfect chords. Easy to play but sounding like an anti-pattern.)

# Anti-Pattern #1 - Having unit tests without integration tests

Summary:

> - The company has no senior developers. The team has only junior developers fresh out of college who have only seen unit tests.
> - Integration tests existed at one point but were abandoned because they caused more trouble than their worth. Unit tests were much more easy to maintain and so they prevailed.
> - The running environment of the application is very "challenging" to setup. Features are "tested" in production.
>
> To sum up, unless you are creating something extremely isolated (e.g. a command line linux utility), you really need integration tests to catch issues not caught by unit tests.

# Anti-Pattern #2 - Having integration tests without unit tests

- It's slow to test all details of a complex application when using slow integration tests.
- Management insists on code coverage. Developers write trivial tests with no value. The tests break easily and require a lot of maintenance.
- It's harder to find the problem when an integration test fails.
- When both unit and integration tests exists, if there's a problem with both unit and integration test, it's obvious where the problem is and what problems it causes.
- It is useful in case of a serious refactoring of code with no unit tests, but only as a temporary measure.

> - Unit tests are easier to maintain
> - Unit tests can easily replicate corner cases and not-so-frequent scenarios
> - Unit tests run much faster than integration tests
> - Broken unit tests are easier to fix than broken integration tests

# Anti-Pattern #3 - Having the wrong kind of tests

What type of a application are you building?

- Example #1: command line utility with CSV input and JSON output. It needs a lot of unit tests, a few integration tests (CSV, JSON) and no GUI tests (because there's no GUI).
- Example #2: payment gateway. Money comes from various sources, it creates a report. Small amount of unit tests, a lot of integration tests, no GUI tests.
- Example #3: website creator with marketplace. A lot of drag/drop, changes (size, color, appearance). A small amount of unit tests, medium amount of integration tests, a lot of GUI tests.

# Anti-Pattern #4 - Testing the wrong functionality

Some functionality is more important than other (critical, core, other). For example, in an online store, is it more important that checkout works, or recommendations? Critical parts should have a lot of tests. Important parts should have some tests. The rest should not have many tests. It's probably better to write another test for critical or important part.

> In summary, write unit and integration tests for code that
>
> - breaks often
> - changes often
> - is critical to the business

# Anti-Pattern #5 - Testing internal implementation

- If more time is spend on fixing tests than on writing code, something is wrong. That leads to "testing is a waste of time" mindset.
- Instead of testing internal implementation, expected results should be tested.
- Example: instead of checking if an object has an attribute and a value (object: user, attribute: type, value: guest, registered, premium) it would be better to check the behavior (guest can see articles, registered user can buy, premium user can collect points).

# Anti-Pattern #6 - Paying excessive attention to test coverage

> If after reading about these metrics, you still insist on setting a hard number as a goal for code coverage, I will give you the number 20%. This number should be used as a rule of thumb and it is based on the [Pareto principle](https://en.wikipedia.org/wiki/Pareto_principle). 20% of your code is causing 80% of your bugs, so if you really want to start writing tests you could do well by starting with that code first. This advice also ties well with Anti-pattern 4 where I suggest that you should write tests for your critical code first.

# Anti-Pattern #7 - Having flaky or slow tests

> In summary, you should have a reliable test suite (even if it is a subset of the whole test suite) that is rock solid. A test that fails in this suite means that something is really really wrong with the code and any failure means that the code must not be promoted to production.

# Anti-Pattern #8 - Running tests manually

> Ideally all your tests should run automatically without any human intervention.
>
> And when I say semi-manual I mean that even though the test suite itself might be automated, there are human tasks for house-keeping such as preparing the test environment or cleaning up the test data after the tests have finished. That is an anti-pattern because it is not true automation. All aspects of testing should be automated.

# Anti-Pattern #9 - Treating test code as a second class citizen

> I have seen projects which have well designed feature code, but suffer from tests with huge code duplication, hardcoded variables, copy-paste segments and several other inefficiencies that would be considered inexcusable if found on the main code.

# Anti-Pattern #10 - Not converting production bugs to tests

I don't think this anti-pattern is absolutely true. You should create tests for some production bugs. But some production bugs are so specific that they will very probably never happen again. Example: database on a Swedish wiki. TODO

# Anti-Pattern #11 - Treating TDD as a religion

When working on a small project, I sometimes make it work first. I don't care if the code is a complete mess. Then, when it works, I enjoy in fixing the code. Usually by writing it from scratch.

# Anti-Pattern #12 - Writing tests without reading documentation first

> Because several developers treat tests as something secondary (see also Anti-pattern 9) they never sit down to actually learn what their testing framework can do. Copy-pasting testing code from other projects and examples might seem to work at first glance, but this is not the way a professional should behave.
>
> Unfortunately this pattern happens all too often. People are writing several “helper functions” and “utilities” for tests without realizing that their testing framework already offers this function either in a built-in manner or with the help of external modules.

# Anti-Pattern #13 - Giving testing a bad reputation out of ignorance

> When I find people like this, it is my hobby to probe them with questions and understand their reasons behind hating tests. And always, it boils down to anti-patterns.