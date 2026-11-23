---
layout: post
title: Just Enough, Just in Time architecture
categories: [architecture, agile]
---

Earlier this year I gave a talk at the Agile Congres in Groningen, in Dutch. I wrote about it [here]({{ site.baseurl }}/architectuur-in-een-agile-wereld/). One part of that talk got the most questions afterwards, so I want to explain it again, in English, and a bit more in depth. It is the idea of Just Enough, Just in Time architecture. I call it JIT-JEA, which is not a very sexy name, but it is easy to remember.

## The false choice

When people talk about architecture in agile teams, they often see only two options.

Option one is Big Design Up Front. An architect spends months on a design. Everything is thought through, everything is documented, everything is reviewed. Then the team can start. The problem: by the time they start, half of the assumptions are already wrong. And changing the design means going back through the review cycle.

Option two is no architecture at all. "We are agile, the architecture will emerge." Sometimes it does. Often what emerges is a big ball of mud, and after two years nobody dares to touch it anymore.

Both options fail. But they are not the only options.

{% include pullquote.html quote="The opposite of Big Design Up Front is not no design. It is smarter design." %}

## Just Enough

The first half is about *how much*. Do only the architecture that the team needs now. Not less, but also not more.

I look at three things:

- **Just enough architecture.** Make the decisions that are hard to change later, and that block the team now. Leave the rest open. If a decision is easy to reverse, don't spend a week on it.
- **Just enough documentation.** Write down what matters. The context, the decision and the reason. One ADR, one or two diagrams. Not a document for the sake of having a document.
- **Just enough governance.** You need some guardrails. But they should guide the team, not stop it. Automated checks in the pipeline work better than a review board that meets once in eight weeks.

How do you know what is "enough"? There is no formula. But a good test is this: can a new team member understand why the system looks like it does, within a day or so? If yes, it is probably enough. If they need a week and three meetings with the architect, it is not.

## Just in Time

The second half is about *when*. Make architecture decisions on the moment you have enough context, and deliver it in small pieces.

This sounds obvious, but it goes against a strong instinct. Architects like to think ahead. We see risks coming, and we want to solve them now. Sometimes that is right. But often we solve problems that never come, and we miss the ones we didn't see.

There is a concept from lean thinking that helps me here: the *last responsible moment*. Don't decide as early as possible. Decide as late as you responsibly can. Late enough that you have real information, early enough that the decision doesn't block the team or become expensive.

In practice this means architecture work follows the rhythm of the team:

| Moment | What you do |
|---|---|
| Refinement | Look at the C4 diagram. Does the new feature still fit? Is there a decision we need to make? |
| During the sprint | Write the ADR when the decision is made, not later |
| Sprint review | Mention what changed in the architecture |
| Retrospective | Which agreements did we not follow? Why not? |

No separate architecture meeting. Architecture becomes part of the normal work.

## What it is not

I want to be careful here, because JIT-JEA can be misused as an excuse. "Just enough" does not mean "as little as possible". And "just in time" does not mean "when it is already on fire".

Some decisions are really expensive to change. Your choice of cloud platform. How you handle identity. How data flows between domains. For those, you need to think early and think well. JIT-JEA doesn't say skip them. It says: spend your up front effort on those, and not on everything else.

It also doesn't mean there is no architect. Somebody needs to keep the bigger picture. Somebody needs to see that three teams are about to solve the same problem in three different ways. That is still the job. But it is done together with the team, and close to the work, not from a distance.

## Getting started

If you want to try this, start small:

1. Pick one decision the team is facing right now. Write an ADR for it, together.
2. Draw a context diagram of your system. Just one. Discuss it in the next refinement.
3. Take one rule that people keep breaking, and turn it into an automated test.

That is it. Do that for a few sprints and see what happens. You can always add more.

Architecture is not a phase at the start of a project. It is something you do a little bit, all the time.
