---
layout: post
title:  "Dependency injection vs Service locator"
date:   2021-01-10 07:00:15 +0530
categories: blog
---


Let’s just first get the basic definition of these 2 patterns clear.

## Dependency Injection:
Dependency injection is nothing but injecting the dependencies to the class needed. **Giving** the class all of its dependencies somehow so that the class which requires those dependencies does not have to worry about those. 

Here’s the Wikipedia definition for Dependency injection
>In software engineering, dependency injection is a technique in which an object receives other objects that it depends on. These other objects are called dependencies. 

Let’s look at some very basic example of how Dependency Injection looks like 

```kotlin
Class Add (int a, int b) {
	return a + b
}
```

Here the class `Add` needs two parameters `a` and `b`. We are supplying those parameters to the class via constructor so that the class add does not have to worry about how to get those dependencies.

<br/>

## Service Locator:
Service locator is also a design pattern where in there will be a central place where we will be having all of the required dependencies dumped and whatever classes want dependencies can **ask** service locator to give those dependencies to them.

Here’s the wikipedia definition for Service Locator
>The service locator pattern is a design pattern used in software development to encapsulate the processes involved in obtaining a service with a strong abstraction layer. This pattern uses a central registry known as the "service locator", which on request returns the information necessary to perform a certain task.

Let’s look at some very basic example of how Service Locator looks like

```kotlin
Class ServiceLocator() {
	fun getClass(className: String): KClass {
		when (className) {
		is “a” -> {
			return A()
		}
		is “b” -> {
			return B()
		}
		is “c” -> {
			return C()
		}
		}
	}
}

Class Add () {
	val SL = ServiceLocator()
	return SL.getClassName(“a”).value() + SL.getClassName(“b”).value()
}
```

Here the class `Add` has an instance of service Locator created inside it and **asks** the service Locator to get all the dependencies (class `A` and `B`) to finish its task.

<br />


## Difference
It both kind of sounds similar and gives us kind of similar benefits  but somewhere you are just wondering why do we have two names for the same pattern which does almost similar work?.

The difference may seem slight here, but even with the ServiceLocator, the class is still responsible for creating its dependencies. It just uses the service locator to do it.  It **asks ServiceLocator** to get its dependencies. With dependency injection, the **class is given** its dependencies. It neither knows, nor cares where they come from. 

<br/>

## Real world example

Consider a you went to a hotel.

**Service Locator:** The service guy comes to you and takes your order and then update your order in the kitchen and brings back the food that you have ordered. Here the service guy acts as a SL, you ask your food to the service locator and he brings those food back to you

**Dependency Injection:** Let’s just imagine that you have been visiting that hotel for years together now and the service guy already knows your order since you have been ordering the same food for the last 1 year or so. Now, as soon as you visit that hotel the service guy knows what you want and directly goes up to the kitchen and brings back your food to the table without even asking you!


Dependency injection seems to adhere to the ["tell don't ask"](https://anirudhuppunda.in/2020/11/tell-dont-ask-principle) principle more than service locators. When you implement dependency injection the class basically **tells** which dependency it requires beforehand. Whereas in service locator, the class **asks** for dependencies.

In short Service Locator and Dependency Injection are just implementations of [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle).

<br />

We can also have the best of both worlds and inject service locator into the required class. If we take the above example from service locator, here is how it looks

```kotlin
Class Add (val SL: ServiceLocator) {
	return SL.getClassName(“a”).value() + SL.getClassName(“b”).value()
}

Add(ServiceLocator())
```

<br />

**Until next time**