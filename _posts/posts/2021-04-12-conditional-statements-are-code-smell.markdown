---
layout: post
title:  "Conditional statements are a code smell"
date:   2021-04-12 02:10:15 +0530
categories: blog
---

Yes, conditional statements are considered [code smell](https://en.wikipedia.org/wiki/Code_smell) if they are used extensively all over the project. alrighty,  let us jump straight to the example to understand **why**.

let's assume we are making a network request to different news sources to get the top headlines from each source. 

``` kotlin
fun getHeadlines(source: String) {
    when (source) {
      "BBC" -> fetchNewsFromBBC()
      "CNN" -> fetchNewsFromCNN()
      "Guardian" -> fetchNewsFromGuardian()
    }
}
```

The problem with the above code is that it violates both [Single-responsibility principle](https://en.wikipedia.org/wiki/Single-responsibility_principle) and [Open–closed principle](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle) principle. I would highly suggest reading [this](https://en.wikipedia.org/wiki/SOLID) if you are not familiar with SOLID principles. Let's understand the problems one by one.

## Problems

1. `getHeadlines` is clearly handling more than one responsibility, currently, it's handling 3 responsibilities.
2. If we want to add a new source in the future we have to touch this piece of logic which we don't want to do. We want this method to be future proof so that even if we need to add few more sources in the future we don't even have to do anything in this method.

<br />

## How can we improve this
We can use one of the OOP concept [Polymorphism](https://en.wikipedia.org/wiki/Polymorphism_(computer_science)) to solve this to make our code cleaner. Let's see **how**


``` kotlin
interface News {
   fun fetchNews()
}
```

``` kotlin
class BBCNews(): News {
    override fun fetchNews() {
       return fetchNewsFromBBC()
    }
}
```

``` kotlin
class CNNNews(): News {
    override fun fetchNews() {
       return fetchNewsFromCNN()
    }
}
```
``` kotlin
class GuardianNews(): News {
    override fun fetchNews() {
       return fetchNewsFromGuardian()
    }
}
```

``` kotlin
fun getHeadlines(newsSource: News) {
    newsSource.fetchNews()
}
```


we can call this function as shown below

``` kotlin
getHeadlines(BBCNews()) // for BBC news
```

``` kotlin
getHeadlines(CNNNews()) // for CNN news
```

``` kotlin
getHeadlines(GuardianNews()) // for Guardian news
```

<br />

## What if i want to add new source?
Got new source?. Just do this

``` kotlin
class CNBCNews(): News {
    override fun fetchNews() {
       return fetchNewsFromCNBC()
    }
}
```

``` kotlin
getHeadlines(CNBCNews()) // for CNBC news
```

<br />

Now `getHeadlines` method is much cleaner to read and is clearly handling single responsibility. Also, we don't need to touch on this method if we want to add a new source in the future. How awesome is that!

<p>so, whenever you see conditional statements which especially operate on type of the object then consider replacing those with OOP concepts and design patterns</p>

<br>

**Untill next time**


