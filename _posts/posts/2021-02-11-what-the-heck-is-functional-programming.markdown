---
layout: post
title:  "What is functional programming"
date:   2021-02-16 07:00:15 +0530
categories: blog
---

Functional programming is a programming paradigm where programs are constructed using few principles. Let's see what those principles are and how it is better than the regular oop.


1. ## Declarative
	1. Focus on what rather than how, we have seen functions like `filter`, `map`, `forEach`. 
	2. These methods focus on **what** it does rather then **how** it does the operation. 

	<br/>

2. ## Pure functions
	1. Function which always gives the same output based on the input. Call this function a thousand times and you will get the same output every single time. 
	2. Pure functions will be independent of the outside world. 
	3. Pure functions will not be having any side effects, no more surprises of figuring out why the boolean was changed when you were calling particular function
	4. Pure functions are easier to test. Since its independent and has no side effects

	<br/>

3. ## Immutability
	1. Your model classes should be immutable
	2. Want something different?. Make a new one

	<br/>

4. ## Concurrent
	1. Pure functions can be executed in any order since those operate on immutable objects and will not have any side effects
	2. No unexpected behaviour

	<br/>

5. ## Higher order functions
	1. Functions are first class citizens
	2. Pass function to a function
	3. Return function as a result from another function

	<br />

**Until next time**
