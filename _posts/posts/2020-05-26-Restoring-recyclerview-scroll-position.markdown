---
layout: post
title:  "Restoring RecyclerView scroll position"
date:   2020-05-26 20:30:00 +0530
categories: blog
---

Most of us might have faced the problem of losing the scroll position of the `RecyclerView` when your `Activity/Fragment` is recreated. This usually happens because the `Adapter` data is loaded asynchronously and data hasn’t loaded by the time `RecyclerView` needs to layout so it fails to restore the scroll position.



## Restoring the scroll position using onRestoreInstanceState
`RecyclerView` already has a solution to this problem. The `LayoutManager` supports `onRestoreInstanceState` method which will help us save scroll positions. 

Let's see small code snippet on how to use this

``` kotlin
private var recyclerScrollPosition: Int? = null
private var recyclerView: RecyclerView? = null

override public fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    recyclerScrollPosition=savedInstanceState.getParcelable("scrollPosition")
}

override fun onSaveInstanceState(outState: Bundle) {
    super.onSaveInstanceState(outState)
    outState.putParcelable("scrollPosition",recyclerView.layoutManager.onSaveInstanceState())
}

```

Once your async operation is done and reattached to `RecyclerView` then call this method
``` kotlin
recyclerView.layoutManager.onRestoreInstanceState(recyclerScrollPosition)
```
<br>

## Why is this solution not recommended anymore?
Even the above solution restores the scroll position of `RecyclerView` whenever `Activity/Fragment` gets recreated but it requires a bit of code and also we must make sure to call `onRestoreInstanceState` at the appropriate position and also should make sure that parcelable state that we pass to the `onRestoreInstanceState`method is not null.

<br>

## Restoring scroll position the official way
Starting from `1.2.0-alpha02`, We don't have to write all these codes just to restore the scroll position. `RecyclerView` offers a new API to let the Adapter block layout restoration until it is ready.

The `recyclerview:1.2.0-alpha02` solution allows you to set a state restoration policy via the StateRestorationPolicy enum. This has 3 options.

1. **ALLOW**
2. **PREVENT_WHEN_EMPTY**
3. **PREVENT**


All we have to do is set the restoration policy on the adapter like this

 ``` kotlin
 adapter.stateRestorationPolicy = PREVENT_WHEN_EMPTY
 ```


Let's see what those options mean and how to use them.

**ALLOW**: This is the default state. It restores the `RecyclerView` state immediately, in the next layout pass.

Since this restores the state immediately this will not be useful if you have an async operation and you are setting the adapter with the result after you get the response. This will be useful if you don't have an async operation and are setting the `RecyclerView` adapter with the result list as soon as you initialize. Below you can see the gif of how it looks when applied to asynchronous vs synchronous operation


`ALLOW` on sync operation  |  `ALLOW` on async operation
:-------------------------:|:-------------------------:
![](https://github.com/AnirudhBhat/RecyclerViewScrollRestorationDemo/blob/master/gifs/allow_demo.gif?raw=true)  |  ![](https://github.com/AnirudhBhat/RecyclerViewScrollRestorationDemo/blob/master/gifs/prevent_async_demo.gif?raw=true)

<br>
<br>

**PREVENT_WHEN_EMPTY**: This one restores the `RecyclerView` state only when the adapter is not empty (adapter.itemCount > 0)

This one is suitable when you have async operation going on because `RecyclerView` state will be restored only after you update your adapter with the data. This can also be used with synchronous operations i.e when you already have the list ready and are populating the adapter with the list during adapter initialization provides the same result. 

In the below gif you can see that I have added a delay to fake the async operation, Once the delay is over I am populating the adapter with the list.

![](https://github.com/AnirudhBhat/RecyclerViewScrollRestorationDemo/blob/master/gifs/prevent_when_empty_demo.gif?raw=true)

<br>
<br>


**PREVENT**:  All state restoration is deferred until you set `ALLOW` or `PREVENT_WHEN_EMPTY`.

When you set this restoration policy while you initialize your adapter then after you get the results and update the adapter, the scroll position will not be restored unless you set either `ALLOW` or `PREVENT_WHEN_EMPTY` while updating your adapter with the result list. So when you choose this option for your adapter then don't forget to update your restoration policy later when you get the results back.  

<br>
<br>
<br>


You can find the complete code in my [Github](https://github.com/AnirudhBhat/RecyclerViewScrollRestorationDemo) repository. 

Basically it has 3 branches apart from master.
1. **recyclerview_sync**: In this branch you can see code which uses `ALLOW` restoration policy.
2. **recyclerview_async**: In this branch you can see code which uses `PREVENT_WHEN_EMPTY` restoration policy. This repo fakes the async operation which helps in getting better understanding.
3. **recyclerview_async_prevent**: In this branch you can see code which uses `PREVENT` restoration policy and how we are updating the restoration policy back again once we get the results.

<br>

**Untill next time**
