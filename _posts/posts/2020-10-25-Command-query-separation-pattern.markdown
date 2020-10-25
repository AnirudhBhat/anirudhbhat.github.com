---
layout: post
title:  "Command-query separation principle"
date:   2020-10-25 15:33:33 +0530
categories: blog
---

This is the wikipedia definition for command query separation principle

> It states that every method should either be a command that performs an action, or a query that returns data to the caller, but not both. In other words, Asking a question should not change the answer.

CQS basically says that your function name **should exactly** do what it says. For example consider the below code

``` kotlin
	 fun fetchNames(): List<String> {
		counter = counter + 1
		// code to fetch names from api
	}   

```

In the above example you can see that the function name `fetchNames` has a side effect of incrementing the counter value (performing action). The function not just returns names list but also changes the state as well. 

Here’s another example

```kotlin
	
	fun saveNamesIntoDB(namesList: List<String>): List<String> {
		// save names list into database
		return namesList
	}

```

Even here the function name tells us that it will be saving names in to database but it also has a return type which causes confusion while reviewing code. This function again has a side effect that it is not just saving the names list in to database as the name implies, it is also returning those names back to the caller.

CQS principle states that either you should have a function which returns the result (no change in state) or function which changes state but will not return anything. But not both.

Adapting this principle might not feel like much benefit in this context but if you are working on large code base this can easily get out of hand and will be source of some of the hard to debug bugs. 

Its always good thing that function does one and only one thing and the function name exactly represents that. 

When you start implementing CQS principle you will begin to realise that your function will become more predictable, no side effects


**Untill next time**
