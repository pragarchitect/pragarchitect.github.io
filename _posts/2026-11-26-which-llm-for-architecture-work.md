---
layout: post
title: "Which LLM is best for architecture work? Wrong question."
categories: [architecture, AI]
---

Which LLM should I use for architecture work? It is a fair question. Every few months there is a new model, and every new model comes with a benchmark that says it is the best. So you start to wonder if you are using the wrong one.

---

<!-- TODO Marcel (optioneel): korte situatie waarin iemand je deze vraag stelde, of waarin je zelf twijfelde welk model je moest gebruiken. -->

My honest answer: for most architecture work, it matters less than you think. The top models are all good enough to write a first ADR, list options for an integration, or find a weak spot in a design. The differences between them are real. But they are small compared to two other things: the context you give, and the question you ask.

{% include pullquote.html quote="A good question to an average model beats a vague question to the best one." %}

## Why benchmarks don't help much

Benchmarks test things like coding puzzles, math and general knowledge. Architecture work is different. There is rarely one correct answer. The best option depends on your team, your budget, your existing systems and the decisions you already made. A model can't know any of that unless you tell it.

So when one model gives a better answer than another, look closely. Often it is not the model. You just asked the second time with more context, or with a sharper question.

I wrote earlier that [your architecture repo is the best prompt you have]({{ site.baseurl }}/your-architecture-repo-is-the-best-prompt/). That is still the biggest lever.

## What does matter when you choose

There are differences that matter. Just not the ones on the leaderboard:

- **Where your data goes.** Are you allowed to paste internal designs into it? Check what your company allows. This rules out more tools than any benchmark.
- **Access to your repository.** A model that can read your ADRs, C4 model and code gives better answers than a smarter model in a chat window.
- **Context size.** Fifty ADRs and a Structurizr model is a lot of text. Can the tool handle all of it?
- **Does it push back?** Some models agree with everything you say. For architecture that is dangerous. You want a sparring partner, not a yes-man. Try it: propose a bad idea and see what happens.
- **What your team already uses.** One tool that everybody knows well beats three tools that nobody knows well.

## A simple test

If you still want to compare, don't trust someone else's benchmark. Take a real decision from your own work, one where you know how it ended. Give each model the same context and the same question. Then read the answer like a proposal from a colleague. Did it ask the right questions? Did it name the trade-offs? Did it see what you missed?

Do this with two or three decisions. It takes an hour or so, and it tells you more than any leaderboard.

And yes, the answer to "which model is best" is: it depends. But now you know on what.
