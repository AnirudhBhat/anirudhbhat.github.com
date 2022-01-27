---
layout: post
title:  "Coroutines"
date:   2022-01-27 04:30:00 +0530
categories: fragments
---




A routine is just a sequence of operations/actions which gets executed one by one in the exact same order as specified.

Co stands for cooperation. A co routine suspend its execution to give other co-routines a chance to execute. So a co-routine is about sharing CPU resources so others can use the same resource.

A coroutine is an instance of suspendable computation. coroutine is not bound to any particular thread. It may suspend its execution in one thread and resume in another one.

Coroutines provide concurrency but not parallelism

async and await are not keywords in Kotlin and are not even part of its standard library.

kotlinx.coroutines is a rich library for coroutines developed by JetBrains. It contains a number of high-level coroutine-enabled primitives that this guide covers, including launch, async and others.

In order to use coroutines, you need to add a dependency on the kotlinx-coroutines-core module

<br/>


## RunBlocking

The name of runBlocking means that the thread that runs it gets blocked for the duration of the call, until all the coroutines inside runBlocking { ... } complete their execution.


<br/>


## Structured Concurrency

Coroutines follow a principle of structured concurrency which means that new coroutines can be only launched in a specific CoroutineScope which delimits the lifetime of the coroutine. In a real application, you will be launching a lot of coroutines. Structured concurrency ensures that they are not lost and do not leak. An outer scope cannot complete until all its children coroutines complete. Structured concurrency also ensures that any errors in the code are properly reported and are never lost.

``` kotlin

val time = measureTimeMillis {
    val one = doSomethingUsefulOne()
    val two = doSomethingUsefulTwo()
    println("The answer is ${one + two}")
}
println("Completed in $time ms")

```


It produces something like this:


``` kotlin

The answer is 42
Completed in 2017 ms

```

<br/>


``` kotlin

fun main() {
    val time = measureTimeMillis {
        // we can initiate async actions outside of a coroutine
        val one = somethingUsefulOneAsync()
        val two = somethingUsefulTwoAsync()
        // but waiting for a result must involve either suspending or blocking.
        // here we use `runBlocking { ... }` to block the main thread while waiting for the result
        runBlocking {
            println("The answer is ${one.await() + two.await()}")
        }
    }
    println("Completed in $time ms")
}

```

Consider what happens if between the `val one = somethingUsefulOneAsync()` line and `one.await()` expression there is some logic error in the code, and the program throws an exception, and the operation that was being performed by the program aborts. Normally, a global error-handler could catch this exception, log and report the error for developers, but the program could otherwise continue doing other operations. However, here we have `somethingUsefulTwoAsync` still running in the background, even though the operation that initiated it was aborted. This problem does not happen with structured concurrency, as shown in the section below.


Let us take the Concurrent using async example and extract a function that concurrently performs `doSomethingUsefulOne` and `doSomethingUsefulTwo` and returns the sum of their results. Because the async coroutine builder is defined as an extension on CoroutineScope, we need to have it in the scope and that is what the coroutineScope function provides:

``` kotlin

suspend fun concurrentSum(): Int = coroutineScope {
    val one = async { doSomethingUsefulOne() }
    val two = async { doSomethingUsefulTwo() }
    one.await() + two.await()
}

```

This way, if something goes wrong inside the code of the concurrentSum function, and it throws an exception, all the coroutines that were launched in its scope will be cancelled.


<br/>


## Scope Builder

runBlocking and coroutineScope builders may look similar because they both wait for their body and all its children to complete. The main difference is that the runBlocking method blocks the current thread for waiting, while coroutineScope just suspends, releasing the underlying thread for other usages. Because of that difference, runBlocking is a regular function and coroutineScope is a suspending function.


``` kotlin

// Sequentially executes doWorld followed by "Done"
fun main() = runBlocking {
    doWorld()
    println("Done")
}

// Concurrently executes both sections
suspend fun doWorld() = coroutineScope { // this: CoroutineScope
    launch {
        delay(2000L)
        println("World 2")
    }
    launch {
        delay(1000L)
        println("World 1")
    }
    println("Hello")
}

```


A coroutineScope in `doWorld` completes only after both are complete, so `doWorld` returns and allows Done string to be printed only after that.


<br/>



## Making coroutine wait through the Job

``` kotlin
val job = launch { // launch a new coroutine and keep a reference to its Job
    delay(1000L)
    println("World!")
}
println("Hello")
job.join() // wait until child coroutine completes
println("Done") 

```

A launch coroutine builder returns a Job object that is a handle to the launched coroutine and can be used to explicitly wait for its completion. For example, you can wait for completion of the child coroutine and then print "Done" string.


<br/>



## Sequential by default

Assume that we have two suspending functions defined elsewhere that do something useful like some kind of remote service call or computation. We just pretend they are useful, but actually each one just delays for a second for the purpose of this example:

``` kotlin

suspend fun doSomethingUsefulOne(): Int {
    delay(1000L) // pretend we are doing something useful here
    return 13
}

suspend fun doSomethingUsefulTwo(): Int {
    delay(1000L) // pretend we are doing something useful here, too
    return 29
}

```

What do we do if we need them to be invoked sequentially — first `doSomethingUsefulOneand` `thendoSomethingUsefulTwo`, and compute the sum of their results? In practice, we do this if we use the result of the first function to make a decision on whether we need to invoke the second one or to decide on how to invoke it.

We use a normal sequential invocation, **because the code in the coroutine, just like in the regular code, is sequential by default.** The following example demonstrates it by measuring the total time it takes to execute both suspending functions:

**Note that concurrency with coroutines is always explicit.**


<br/>



## Coroutine context and dispatchers

Coroutines always execute in some context represented by a value of the `CoroutineContext` type, defined in the Kotlin standard library.

The coroutine context is a set of various elements. The main elements are the Job of the coroutine, which we've seen before, and its dispatcher.


The coroutine context includes a coroutine dispatcher that determines what thread or threads the corresponding coroutine uses for its execution. The coroutine dispatcher can confine coroutine execution to a specific thread, dispatch it to a thread pool, or let it run unconfined.

All coroutine builders like launch and async accept an optional `CoroutineContext` parameter that can be used to explicitly specify the dispatcher for the new coroutine and other context elements.


When launch { ... } is used without parameters, it inherits the context (and thus dispatcher) from the CoroutineScope it is being launched from. In this case, it inherits the context of the main runBlocking coroutine which runs in the main thread. That's why, if you use launch coroutine builder directly from the main thread. The launch coroutine builder will be executing in the main thread as well.


<br/>




## Children of a coroutine

When a coroutine is launched in the CoroutineScope of another coroutine, it inherits its context via `CoroutineScope.coroutineContext` and the Job of the new coroutine becomes a child of the parent coroutine's job. When the parent coroutine is cancelled, all its children are recursively cancelled, too.

However, this parent-child relation can be explicitly overriden in one of two ways:

1. When a different scope is explicitly specified when launching a coroutine (for example, GlobalScope.launch), then it does not inherit a Job from the parent scope.

2. When a different Job object is passed as the context for the new coroutine (as show in the example below), then it overrides the Job of the parent scope.

In both cases, the launched coroutine is not tied to the scope it was launched from and operates independently.


``` kotlin

// launch a coroutine to process some kind of incoming request
val request = launch {
    // it spawns two other jobs
    launch(Job()) { 
        println("job1: I run in my own Job and execute independently!")
        delay(1000)
        println("job1: I am not affected by cancellation of the request")
    }
    // and the other inherits the parent context
    launch {
        delay(100)
        println("job2: I am a child of the request coroutine")
        delay(1000)
        println("job2: I will not execute this line if my parent request is cancelled")
    }
}
delay(500)
request.cancel() // cancel processing of the request
delay(1000) // delay a second to see what happens
println("main: Who has survived request cancellation?")

```


The output of this code is:

``` kotlin
job1: I run in my own Job and execute independently!
job2: I am a child of the request coroutine
job1: I am not affected by cancellation of the request
main: Who has survived request cancellation?
```

<br />


Most of the above are directly copied from the [Kotlin official documentation page](https://kotlinlang.org/docs/coroutines-guide.html).

<br />

**Untill next time 👋**

