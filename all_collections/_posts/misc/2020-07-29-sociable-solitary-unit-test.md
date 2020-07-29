---
layout: post
title:  "Unit test: sociable or solitary"
author: "Will Xing"
categories:
  - blog
date: 2020-07-29T00:00:00Z
---

Most developers nowadays are doing TDD, as for me, I have been doing TDD for years. Also I've been TDD with different people and heard about different thoughts about test.

## Test Pyramid

![~Test Pyramid~](/assets/images/traditional-pyramid.png)

The idea of test pyramid is the type of tests in the bottom will cost more and also more isolated.

This is easy to understand, just think about when e2e test happened, it's actually going throw the whole application to do human like operation, test every thing in real, all things should linked together, `http` connection should be real connection, database CRUD also will really happened, it's expensive as a real running product. It's not hard to image that e2e will took much longer time to run.

Also e2e test is not easy to write, especially for web application e2e test, a developer can spend an hour to find out all selectors, finish scenario and pass the test.

And it's very hard to maintain, once the interface changed or feature changed, the test and scenario all need to rewrite/fix.

On the other end, the unit tests, they are all tiny and focus on only a block of function. So they can run very fast, and usually can write more unit test than other tests.

## Socilable or Solitary

For unit test, I usually get into the conversation about "Should we mock/stub this?", well, that's really depends, I think when we find it's more pain to mock or stub than use the real external, it's the time we need to take a look at the code and see why this happened, and should we just use the real external resources?

What I don't think it's so good is to make unit tests as socilable as possible.

Firstly, this will turn unit test to become semi integration test, which blur the boundary of unit test and integration tests. Which will be a bad thing, that can slow down our minimal tests, also increase the number of high cost tests.

And when we do TDD as a developer, we usually will start with unit test, if unit test becomes very big with many side effects, it's much much easier to break and also harder to write. This can slow down development a lot.

I will like to write unit tests more solitary.

Actually, as solitary as possible, only when it's cost too much to mock things, should step back and think over the design and why this happen.

The benefit will be just opposite of the cons above when unit test it's too socilable, and it's much faster to develop and much easier to find issue with isolated unit tests.

## Conclusion

Test Pyramid is useful and make sense, that's why developer better main good balance and clear boundary between each level of tests, it does have some gradularity inside this pyramid, the over all direction won't change much.