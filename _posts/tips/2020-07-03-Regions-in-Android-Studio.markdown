---
layout: post
title:  "Regions in Android Studio"
date:   2020-07-3 19:10:00 +0530
categories: tips
---

When classes become quite large (Always try to have a class with less than 200 lines :) ), it becomes hard to focus on certain piece of logic without all the noise/distraction from other code present in the same file. To solve this we can use what is called as `Regions` in Android studio.


## Regions
As the name says it basically helps in creating regions in android studio. We can have regions for click listeners, Global variables, UI setup, Lifecycle methods..etc. We can create regions in Android studio like shown below

```
//region UI setup
	setupRecyclerView()
	setupTabs()
	 …etc
//endregion
```

This is how regions looks when collapsed in Android studio

![image](https://github.com/AnirudhBhat/anirudhbhat.github.com/blob/master/assets/regions_android_studio.png?raw=true)

## Keyboard Shortcuts
Command + shift + plus (+) expands the entire file

Command + shift + minus(-) collapses the entire file

If you want to expand or collapse a certain region then

Command + plus(+) expands region

Command + minus(-) collapses region
