---
layout: post
title: The architect asks questions, not always answers
categories: [architecture, teams]
---

A team came to me with a simple question: "Should we use REST or messaging for this integration?" I could have answered it in one minute. I have an opinion about it, of course. Every architect has.

---

But I didn't answer. I asked back: "What happens when the other system is down for an hour?"

It was quiet for a moment. Then the discussion started. Twenty minutes later the team had their answer, and it was a better one than mine. They knew things about the other system that I didn't know.

{% include pullquote.html quote="If the architect gives all the answers, the team stops thinking." %}

When you are the architect, people expect you to know. And it feels good to know. You give the answer, the team goes on, everybody is happy. But there is a hidden cost. The team learns that architecture is something that comes from you. Next time they will wait for you again. And you become the bottleneck that you never wanted to be.

A good question does more work than a good answer. It moves the thinking to the people who will build it, and who will live with the result.

Some questions I use a lot:

- What happens when this fails?
- Who needs to change this in two years?
- What is the cheapest thing we can try first?
- What would make us regret this decision?
- How do we know it works?

This doesn't mean you never give an answer. Sometimes the team is stuck, or time is short, or you simply have experience they don't have yet. Then say it. Being pragmatic also means not playing games.

But try the question first. Most of the time, the team is closer to the answer than you think.
