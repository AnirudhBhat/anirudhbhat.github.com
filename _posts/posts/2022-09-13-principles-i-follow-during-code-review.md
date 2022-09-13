---
layout: post
title:  "Principles I follow during the code review"
date:   2022-09-13 16:40:00 +0530
categories: blog
---

**Empathy during the code review:** This is a big one for me. Always keep in mind that you are reviewing the code and not the author. I've been on the other side to know how it feels when the reviewer is not empathetic. If you think some piece of logic can be done in a better way. Provide concrete examples and **explain** why you think your example is better than theirs. Also, this should never be forced upon the author and act as a blocker for PR merge. 

<br/>

**Automate nitpicks:** It's waste of time for both the reviewer and the author to nitpick on tiny things. Catching these things should be automated.  

<br/>

**Use emojis to let the author know your intentions:** Sometimes words get misunderstood. I feel it's better to convey our intentions via an emoji.

 - 🔍 Nitpick/Not important at all
 - ❓ I’d like to understand this more, but definitely not blocking the PR
 - 💡 An idea how to possible improve the code (the fact that it improves the code is just my opinion)
 - ⚠️ Something I’d really like to discuss before merging the PR - still not a blocker, but something I care about
 - ⛔ Blocker - I think this could affect users. Can we please discuss this before we merge the PR.

 I've copied this one from my colleague.

 <br/>

**Blockers are only those that have some sort of issue with the code:** I consider below as blockers and I would love to discuss before merging the PR

 - Doesn't work as mentioned in the PR description
 - Has unintended side effect

Rest everything are your opinions. Your opinions MUST never be the reason for blocking the PR from merge. Use proper emojis to suggest the changes and leave the decision to the author. Approve the PR.

<br/>


**Appreciate if you see something that is done beautifully:** Last but not least, appreciate if you see something that the author has done well. Be it as small as adding a comment like "Nice" or adding an emoji. 

<br />

**Untill next time 👋**