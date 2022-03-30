---
layout: post
title:  "Coroutine context vs Coroutine scope"
date:   2022-03-30 12:45:00 +0530
categories: fragments
---




There are two instances of coroutine context available. One is from the CoroutineScope and the other is an instance from CoroutineContext itself.

When we use coroutine builders like "launch" or "async", we get 2 coroutine context. One from coroutinescope and another from the coroutine builder.

coroutine builders take "coroutine context" as one of the parameters.

coroutine builder merges its coroutine context with its scope's coroutine context. However, the coroutine builder's coroutine context gets higher priority.

The context we get from merging both the context becomes the parent for the newly launched coroutine. 

coroutineScope { // context from coroutine scope: a
    launch() { // context from coroutine builder: b
    ....... // This coroutine builder will have a+b with b taking precedence, as its parent context.
    }
}

From the above code, coroutine builder can add its own element to the context and it overwrites the scope's context elements.


Job


coroutineScope { // Job from coroutine scope: a
    launch() { // child job from coroutine builder with a as its parent: b. i.e a ---> b
    ....... // Observe the parent-child relationship between scope's job and coroutine builder's job. This is the reason cancelling the parent job cancells all the children jobs as well since parent //has reference to all of its children.

    // also, the context for coroutine builder will consist of new job instance. i.e b, along with the parent(a) context
    }
}


coroutineScope { // Job from coroutine scope: a
    launch(new Job b) { // coroutine builder explicitly creates a new job b
    ....... // Here you are basically breaking the parent-child relationship by //creating explicitly a new job for coroutine builder. If you cancel the parent scope now, the child will not be cancelled.
    }
}

In the above code, if you replace coroutineScope with GlobalScope, you loose the control of canceling all the child coroutines since GlobalScope doesn't return any job. That's what it means, GlobalScope runs globally and terminates only when our process terminates.


The intended purpose of CoroutineScope receiver in launch and in all the other coroutine builders is to reference a scope in which new coroutine is launched

The intended purpose of context: CoroutineContext parameter in launch is to provide additional context elements to override elements that would be otherwise inherited from a parent scope


<br />

**Untill next time 👋**