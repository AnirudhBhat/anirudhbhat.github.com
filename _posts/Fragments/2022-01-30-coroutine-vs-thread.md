---
layout: post
title:  "Coroutine vs Thread"
date:   2022-01-29 02:12:00 +0530
categories: fragments
---

Coroutines are similar to threads. Switching decision of the Coroutines are designed in the programming language and the programmer controls when the switch happens whereas switching of the threads are designed in the kernel level by some scheduling algorithm

Coroutines never run in parallel or take advantage of multiprocessors whereas threads run parallelly and make use of processors

Threads consume resources. Every thread will be having its own stack. Because of this, it's costlier to switch threads. Since every thread will be having its own stack, threads also consume more memory. The number of threads created will increase memory usage linearly

Coroutines don't involve system/kernel APIs and hence they are not costlier to switch. Also, the memory footprint is less. The number of Coroutines created is not directly proportional to memory usage

Coroutine is not bound to any particular thread. It may suspend its execution in one thread and resume in another one.