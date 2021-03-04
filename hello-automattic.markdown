---
layout: page
title:  "Hello Automattic! 👋"
permalink: /hello-automattic/
---

# Hello Automattic! 👋

<p>I'm very happy to express my interest in joining your team as a Mobile Engineer!</p>

<p>I've been following Automattic since long time and have read your blogs and watched Matt Mullenweg's interviews. Every time i read one of your blogs or use your product my interest to join Automattic has only been increasing</p>

<p>I will give you 3 reasons for hiring me, i will explain my motivation, and i will list the projects that i have worked on</p>

*First*, I can write clean code that solves complex problems. I always believe that design patterns, SOLID principles and architecture are far more important than the technology or the language we use to solve the problem. I like to keep things as boring and simple as possible. Technology is a means and not an end.

*Second*, I write tests and an extensive believer in writing good unit tests. In my opinion any software which does not have tests are legacy software

*Third*, I am a team player. I always try to give positive feedback during code reviews and does not nitpick on small things which really aren't important on the higher level. I love to work with people who show empathy and kindness. I have been in a situation where i understand being calm and showing kindness goes a long way.

and why Automattic? All these years most of my work has been closed source except for my side projects which are available on Github. I really amire that all of the Automattic's android apps are open source and i also
love the company's open source first approach. I also love the way the company is run, the philosophy behind it and the hiring ethics.


## Projects
**Noon academy** [Play store link](https://play.google.com/store/apps/details?id=com.noonEdu.k12App&hl=en_IN) 

**Role:** Senior android engineer

**Contributions:** 
1. Introduced team to writing Unit tests. Gave internal talks about testing.
2. Wrote custom lint checks to get a consistent design throughout the app
3. Gave internal talk on MVI architecture and its benefits with respect to testing
4. Was part of 3 major feature module from scratch and 1 major rewrite

**frameworks/Architecture**
1. MVVM for some of the module and MVI for few others
2. Retrofit for networking
3. LiveData for reactive stuff
4. Dagger2 for DI
5. java/kotlin

**Learnings:** 
1. It’s impossible to scale an app without proper design system in place.
2. Rewriting an old feature with zero unit tests and get the requirement’s right is quite hard. Knew this beforehand but experienced it first hand

<br/>

**Bytemark**
[NYW](https://play.google.com/store/apps/details?id=co.bytemark.nywaterway&hl=en_US),
[MyDart](https://play.google.com/store/apps/details?id=co.bytemark.mydart&hl=en_US),
[SouthShore](https://play.google.com/store/apps/details?id=co.bytemark.southshore&hl=en_US),
[Ripta](https://play.google.com/store/apps/details?id=co.bytemark.ripta&hl=en_US)

**Role:** Android engineer

**Contributions:** 
1. Introduced team to writing Unit tests
2. Initiated using Kotlin for new features and tests
3. Worked with a German company to integrate their sdk into our app

**frameworks/Architecture**
1. MVVM for some of the module and MVI for few others
2. Retrofit for networking
3. RxJava for reactive stuff
4. Dagger2 for DI
5. java/kotlin

**Interesting challenge that I faced:** our iOS apps were getting rejected by apple for some weird reason. My then colleague (Now my wife :)) and I sat together to figure out why apple was rejecting our apps and later found out it was because of slow startup time. Apple has this set startup time within which all apps should start and be ready to use. Our apps were failing to start within that time on some of the iPhones and that too it wasn’t quite reproducible always. We checked the code written in our splash screen and everything looked fine, calls were being made on background thread. After some time we figured out that we were using a SDK (Remember the SDK that I mentioned above?) which was being initialised on app startup and it internally was setting up maps and few other stuff which was resulting in slow startup times. 

Although this was not that complex to figure out it was fun doing this.

**Learnings:** it was here that I realised why fakes were better than mocks when unit testing. Eye opener

You can find my open source side projects from my [Github](https://github.com/AnirudhBhat/)

<br/>
Let's talk!

