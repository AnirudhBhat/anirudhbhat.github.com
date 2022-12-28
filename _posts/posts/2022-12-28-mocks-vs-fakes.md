---
layout: post
title:  "Mocks vs Fakes"
date:   2022-12-28 11:17:00 +0530
categories: blog
---

I have read couple of blog posts lately about the discussion around using mocks vs fakes in the android community. I remember reading same kind of argument few years back as well. There is a strong opposition for using mocks and all of them suggest to use fakes instead.

Here are some of the arguments for using fakes over mocks
1. Mocks encourage or make it easy to expose implementation details in the unit tests.
2. Fakes are generally faster than mocks since there’s no reflection in play here.
3. No need of third party dependencies

I have a decent amount of experience working on codebases that had used both fakes and mocks for testing. In my current organisation, we don’t use fakes much. We mostly use mocks for writing unit tests.

Around 2-3 years back, when I first read the discussions about fakes vs mocks, I blindly believed what I read and thought fakes are the way to write tests, writing fakes makes you know your stuff, real developer always write fakes over mocks…etc

It was only until I worked on the codebase that used mocks extensively all over the place for unit testing that made me pause for a bit and think for myself that mocks are not bad, there’s nothing wrong in using mocks instead of fakes if it’s serving well for your team.

Each has it’s own pros and cons. What really matters is whether that con is really making you go slow, if the approach you took makes the codebase hard to refactor, does your tests fail for small behaviour change in the code.

Going again over the arguments for using fakes over mocks again:
**Mocks tie implementation details in the unit tests:** Then don’t do this. As simple as that. You can write unit test using mocks that don’t bother about the implementation details and just tests the behaviour. It all depends on how you write tests and has got very little with the tools you use.

**Fakes are generally faster than mocks:** Maybe, is that noticeable? is your productivity going down because of that? Do you think those milliseconds are worth the time refactoring the codebase from mocks to fakes? If yes, then please go ahead and do that. Else, don’t bother and it’s not worth it.

**No need of third party dependencies:** Yes. This depends on your priority. Do you think if adding a dependency in your codebase for unit testing is not worth it, then use fakes. If it’s no big deal and you have no problems with using mocks then continue using it.

What I’m arriving at is that **I don’t care whether the codebase uses fakes or mocks as long it has extensive unit tests written**. You don’t need to either. It’s nothing wrong to use mocks if that’s working for you fine. Just because someone famous on online said that fakes are better than mocks, you don’t have to follow the same.  Everyone’s priorities are different and what works for them might not necessarily work for you. Don't fix something that isn't broken.

Just follow whatever works and ensure you write extensive tests regardless of whichever approach you pick.

The Bottom line is **Just ensure you write extensive tests and everything else is just noise.** Happy testing!


<br />

**Untill next time 👋**

