---
layout: post
title: Threat modeling for teams that have no time
categories: [architecture, security]
---

Last year I did some sessions on threat modeling, including a workshop with a team that builds software for a government organisation. One thing I heard more than once: "We know we should do threat modeling. But we don't have time for it."

---

I get it. When people hear "threat modeling" they think of a two-day workshop with a security specialist, a huge spreadsheet with every possible threat, and a report that nobody reads afterwards. If that is the picture, then yes, no team has time for that.

But threat modeling can be much smaller. And honestly, a small threat model that you actually do is better than a perfect one that stays on the planning.

{% include pullquote.html quote="A small threat model you actually do beats a perfect one you keep postponing." %}

## Four questions

The simplest way I know is the approach of Adam Shostack, with four questions:

1. **What are we working on?**
2. **What can go wrong?**
3. **What are we going to do about it?**
4. **Did we do a good job?**

That's it. You can do this in an hour, with the team, at a whiteboard.

## What are we working on?

Draw the system. You probably already have a C4 container diagram, so use that. Then add two things: where the data flows, and where the trust boundaries are. A trust boundary is every place where data goes from one level of trust to another. From the browser to your API. From your API to an external service. From the internet into your network.

Most of the interesting threats live on those boundaries.

## What can go wrong?

Now walk through each boundary and ask what can go wrong. To help the team think, I use STRIDE as a checklist:

- **S**poofing: can someone pretend to be someone else?
- **T**ampering: can someone change data they shouldn't?
- **R**epudiation: can someone deny they did something?
- **I**nformation disclosure: can data leak?
- **D**enial of service: can someone make it unavailable?
- **E**levation of privilege: can someone get more rights than they should?

You don't need to be complete. Write down what the team comes up with. Usually after half an hour you have a list of ten to twenty things, and a few of them make people a bit nervous. Those are the ones you want.

## What are we going to do about it?

For each threat, pick one: fix it, reduce it, accept it, or move it to someone else. Not everything needs a fix. But accepting a risk should be a choice, not something that happens because nobody looked.

The fixes go on the backlog like any other work. The accepted risks I like to write down in an ADR, so you know later why you didn't do something.

## Did we do a good job?

Look back after a few sprints. Did the fixes get done? Did we miss something? Did a pen test or an incident find something we should have seen?

## Make it a habit, not an event

The real trick is to not do it once. Do a small threat model every time something important changes. A new integration, a new kind of user, new sensitive data. Put it in the definition of done for those kinds of stories. Then it becomes normal work, like writing tests.

One hour, four questions, the whole team. You do have time for that.
