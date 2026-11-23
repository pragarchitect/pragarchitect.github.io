---
layout: post
title: "Architecture choices are business choices"
categories: [architecture, teams]
---

"This is a technical decision. The business doesn't need to be involved." I hear this a lot, from developers and from product owners. I understand why people say it. But it is almost never true.

---

Earlier this year I touched on this in my talk at the Agile Congres, which I wrote about [in Dutch]({{ site.baseurl }}/architectuur-in-een-agile-wereld/). It was only one slide. I think it deserves a bit more.

<!-- TODO Marcel: korte anekdote over een "technische" keuze die later een duidelijke zakelijke impact bleek te hebben (kosten, leverancier, doorlooptijd) -->

Every architecture choice has a price and a risk. Use a managed database, and you pay every month, but nobody has to patch it at night. Build on one vendor's platform, and you move fast now, but leaving later is expensive. Split a system into services, and teams can work more independently, but you also need people who can run all those services.

None of this is purely technical. It is about money, time, risk and dependency. And that is exactly what the business cares about.

{% include pullquote.html quote="If a choice has a price and a risk, it is a business choice. That is almost every architecture choice." %}

## Different words, same problem

The business does care. They just use other words.

| The business says... | The architect hears... |
|---|---|
| "It takes way too long" | Scalability. We grow faster than the architecture can handle. |
| "It costs more every year" | Technical debt. The price of earlier choices. |
| "We depend on that one vendor" | An integration choice. Easy back then, risky now. |

So part of the job is translation, in both directions. Without it, business and IT talk past each other, and the architect makes choices that are really not theirs to make alone.

## How to bring the business in

- **Talk about time, money and risk.** Not about frameworks or patterns.
- **Give options with a price tag.** "Option A is ready in two weeks, but it locks us in. Option B takes six weeks, but we can switch later." Then let them choose.
- **Ask what matters most right now.** Speed, cost or flexibility. The answer often changes the architecture.
- **Write it down.** In an ADR, add at least one consequence a product owner can understand. I explain how in [my practical guide to ADRs]({{ site.baseurl }}/a-practical-guide-to-adrs/).

## When it really is just technical

Some decisions are just technical. Which logging library. How you name your packages. Don't take those to a steering committee, you will only waste everybody's time.

A simple test: if a choice is cheap to change and doesn't affect cost, risk or a vendor, decide it in the team. If it is expensive to change, or someone will see it on an invoice, bring the business in.

The business will not always choose what you would choose. That is fine. It is their money. Your job is to make sure they know what they are buying.
