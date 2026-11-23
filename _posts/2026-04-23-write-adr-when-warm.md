---
layout: post
title: Write the ADR while the decision is still warm
categories: [LessonsLearned,Architecture]
---

Last week a developer asked me why we use a message queue between two services. Good question. I knew we decided it together, about a year ago, in a meeting with the whole team. But the reason? I could only give half of it. And the ADR for it was never written.

---

We all planned to write it "later". Later never came.

I see this happen in almost every team I work with. The team makes a decision in the middle of a sprint. Everybody agrees, everybody understands why. So it feels like there is no need to write it down. It is obvious, right?

It is obvious on that moment. Not six months after.

{% include pullquote.html quote="A decision you don't write down is a decision you will make again." %}

The problem is that the context of a decision fades very fast. Which options did we look at? Why did we not pick the other one? What was the constraint that made the difference? Maybe it was the budget, maybe the deadline, maybe a colleague who knew Kafka very well and then left the company. If you don't capture that, the next team will discuss the same thing again. Or worse, they will undo your decision without knowing why it was there.

So my rule is simple: write the ADR in the same sprint as the decision. Better, in the same pull request as the code change. Then the reviewer sees the code and the reasoning together.

And it doesn't have to be perfect. An ADR of ten lines that exists is worth more than a beautiful one that is still on someone's to-do list. Context, options, decision, consequences. That is enough. You can always add more later. (Yes, I know what I just said about later.)

Some things that help in my teams:

1. Make it part of the definition of done. No ADR, no done.
2. Let the person who did the research write the first version. The architect can review, not the other way around.
3. Keep the template small. If it takes more than fifteen minutes, people will skip it.

In the end ADRs are not about documentation. They are about respect for the people who come after you. And quite often, that person is you.