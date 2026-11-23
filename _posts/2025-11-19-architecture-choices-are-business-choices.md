---
layout: post
title: Architecture choices are business choices
categories: [architecture, business]
---

There is a misunderstanding I meet quite often. Architecture is technical, so the product owner and the business don't need to be involved. The architect and the developers will sort it out.

But every architecture choice has a price and a risk. And price and risk are always business things.

Choose a managed cloud service, and you pay a monthly bill and you depend on a vendor. Build it yourself, and you pay in development time and in maintenance for years. Postpone the refactoring, and you pay later with slower delivery. None of these are purely technical. Somebody with a budget should have an opinion about them.

{% include pullquote.html quote="If the business doesn't understand your trade-off, you didn't explain it well enough." %}

The tricky part is that business and tech often describe the same problem in different words. Some examples:

| The business says... | The architect hears... |
|---|---|
| "Everything takes so long" | Scalability, or a system that is hard to change |
| "It costs more and more money" | Technical debt, the price of earlier choices |
| "We depend on that one supplier" | An integration choice that was easy then and risky now |
| "We can't move fast enough" | Coupling between teams and systems |

Part of the architect's job is to translate. In both directions. Explain to the business what a technical choice will cost them. And explain to the team why the business is worried, because "the PO is difficult again" is usually not the real story.

A practical way to do this: in your ADRs, add a short section about the consequences in plain language. Not "increased coupling", but "if we pick this, team B can not release without team A". The product owner can read that. And then they can actually make a choice.

Architecture is not something that happens next to the business. It is the business, in a technical form.
