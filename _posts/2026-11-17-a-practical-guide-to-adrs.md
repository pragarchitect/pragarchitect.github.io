---
layout: post
title: "A practical guide to Architecture Decision Records"
categories: [architecture, ADR]
---

I have written about ADRs a few times now. In the [Java Magazine article]({{ site.baseurl }}/architecture-as-code-java-magazine/), in my [Agile Congres recap]({{ site.baseurl }}/architectuur-in-een-agile-wereld/), and in a short post about [writing them while the decision is still warm]({{ site.baseurl }}/write-the-adr-while-it-is-still-warm/). Every time, I get the same kind of questions afterwards. Which template do you use? Where do you put them? What do you do when a decision changes? Who writes them?

So this is the long version. Everything I learned about ADRs in practice, in one place. It is not the only way to do it. But it is a way that works for the teams I work with.

## What an ADR is (and what it isn't)

An Architecture Decision Record is a short document that captures one architecture decision. Not a design, not a specification. One decision, with the context and the reasons.

The idea comes from Michael Nygard, who wrote about it in 2011. His point was simple: in agile teams, nobody reads big documents, but people do want to know why things are the way they are. So write small records of the decisions, and keep them with the code.

What makes something an *architecture* decision? A good definition I use: it is a decision that is hard or expensive to change later. Choosing a database. Choosing between sync and async communication. Deciding how you split your services. Deciding how authentication works.

What is not an ADR: the name of a variable, which logging library to use for one module, the colour of a button. Those are decisions too, but they are easy to change. Don't write ADRs for them, or people will stop reading them.

{% include pullquote.html quote="If it is easy to change later, it probably doesn't need an ADR." %}

When in doubt, ask the team: "Will someone in two years wonder why we did this?" If the answer is yes, write it down.

## A template that people will actually use

There are many templates. Nygard's original one, MADR, Y-statements, and many company-specific ones. My advice: start with something small. A template that takes more than fifteen minutes to fill in will not get filled in.

This is roughly what I use, based on MADR:

```md
# 0007 Use PostgreSQL for the order service

* Status: accepted
* Date: 2026-03-12
* Deciders: team Orders, Marcel (architect)

## Context and problem statement

The order service needs to store orders and their status history.
We expect around 50.000 orders per day, growing. Reporting needs
to run queries over several months of data. The team has good
experience with relational databases, and our platform team
offers managed PostgreSQL.

## Considered options

* PostgreSQL (managed, via platform team)
* MongoDB
* Keep using the shared Oracle database

## Decision

Chosen option: PostgreSQL, because it fits the relational nature
of the data, supports the reporting queries well, and is offered
as a managed service so we don't need to run it ourselves.

## Consequences

* Good: no operational work for the team, known technology
* Good: we are no longer blocked by changes on the shared Oracle schema
* Bad: we need a migration of existing orders from Oracle
* Bad: reporting team needs access to a new database

## More information

Migration plan: see ticket ORD-412.
Related: ADR-0003 (split order service from the monolith)
```

A few things I want to point out.

**The title is the decision.** Not "Database choice", but "Use PostgreSQL for the order service". Then the list of ADRs reads like a list of decisions, and you often don't need to open the file.

**Context is the most important part.** The decision itself is usually one sentence. The context explains the situation on that moment: the numbers, the constraints, the skills in the team, the deadline. This is the part that will be gone from everyone's memory in a year. It is also the part that tells a future reader if the decision is still valid. If the context changed, maybe the decision should change too.

**Write down the options you didn't pick.** This saves a lot of discussions later. When someone says "why didn't we just use MongoDB?", the answer is already there.

**Consequences, good and bad.** Be honest. Every decision has downsides. If you only write the good ones, the ADR looks like a sales pitch and people will not trust it. I also like to write at least one consequence in plain language that a product owner can understand. See also my post on [architecture choices being business choices]({{ site.baseurl }}/architecture-choices-are-business-choices/).

## Where to keep them

In the repository, next to the code. I usually use a folder `docs/adr/` in the root.

One file per decision, with a number in front:

```
docs/adr/
  0001-record-architecture-decisions.md
  0002-use-hexagonal-architecture.md
  0003-split-order-service-from-monolith.md
  0004-use-kafka-for-order-events.md
  ...
```

The number gives you a stable reference ("see ADR-0004") and keeps them in order. Use four digits, you will be surprised how fast you reach fifty.

The first ADR is always the same: "Record architecture decisions". It explains that the team uses ADRs, which template, and where they live. It is a bit of a joke, but it is also useful for new team members.

What if you have many repositories? Then I make a split. Decisions about one service go in that service's repository. Decisions that go over multiple services or teams go in a separate architecture repository. Link between them where needed. It is not perfect, but it is better than one huge central list that nobody feels responsible for.

## Statuses and the lifecycle of a decision

An ADR has a status. I use these:

- **Proposed** – the decision is being discussed. The ADR is in a pull request.
- **Accepted** – the decision is made.
- **Deprecated** – the decision is not relevant anymore, for example because the system was removed.
- **Superseded by ADR-XXXX** – a newer decision replaces this one.

The most important rule: **never change the content of an accepted ADR.** If the decision changes, write a new ADR and mark the old one as superseded. Add a link in both directions.

This feels like extra work at first. Why not just update the old one? Because the history is the value. You want to see that in 2025 the team chose Kafka for good reasons, and that in 2027 they moved away from it for other good reasons. If you overwrite the first decision, you lose that story. And you lose the chance to learn from it.

Small fixes like typos or adding a link are of course fine.

## Who writes them, and when

Not the architect. Or at least, not only the architect.

The best ADRs I have seen were written by the developer who did the research. They know the options, they tried things, they know the details. The architect can help with the structure, ask the hard questions in review, and make sure the bigger picture is covered.

{% include pullquote.html quote="The ADR is not the architect's homework. It is the team's memory." %}

As for when: in the same sprint as the decision. Better, in the same pull request as the code change. Then the reviewer sees both. And if the decision needs discussion, open a pull request with only the ADR in status "proposed". The pull request becomes the place for the discussion. When it is merged, the status goes to accepted.

This also means you don't need an architecture review board for most decisions. The pull request is the review. People who care can comment. Of course, for really big decisions you may want more people to look at it. Then tag them in the pull request.

## Common mistakes

Some things I have seen go wrong, sometimes in my own teams.

**Writing them afterwards.** "We will document the decisions at the end of the project." You won't, and if you do, the context is already gone. You get ADRs that say what was decided but not why.

**Too big.** An ADR of five pages is a design document with a different name. Keep it to one decision. If you need more space, link to a separate document.

**Too many.** If every small choice becomes an ADR, people stop reading them. Then the important ones get lost between the unimportant ones.

**Only the happy consequences.** Already mentioned it, but it is the most common one. If there are no downsides in your ADR, you didn't think hard enough. Or you didn't want to write them down, which is worse.

**Nobody knows they exist.** ADRs are only useful when people read them. Mention them in refinements. Link them in pull requests. Point new colleagues to the folder on their first day.

**The architect writes them all.** Then the team sees them as the architect's thing, and the ADRs slowly drift away from reality.

## Tooling

You don't need much. A Markdown file and a pull request is enough.

If you want a bit of help, there is [adr-tools](https://github.com/npryce/adr-tools), a small command line tool that creates numbered files and handles the superseded links. [Log4brains](https://github.com/thomvaill/log4brains) can generate a nice static website from your ADRs, which is useful if non-developers also want to read them. And Structurizr can include ADRs next to your C4 diagrams, so architecture and decisions live together.

But honestly, start without tools. Add them when you feel the pain.

## ADRs and AI

One last thing. Since ADRs are plain Markdown in the repository, AI tools can read them. That turns out to be very useful. An agent can check if a new change goes against an earlier decision, or write a first draft of an ADR based on a pull request. I wrote more about that in [Your architecture repo is the best prompt you have]({{ site.baseurl }}/your-architecture-repo-is-the-best-prompt/).

It's another reason to write them. Not only for your future colleagues, but also for the tools you use today.

## Start tomorrow

If your team doesn't use ADRs yet, here is how to start:

1. Create `docs/adr/` in your repository.
2. Write ADR-0001 "Record architecture decisions" together with the team.
3. Pick the next real decision the team needs to make, and write ADR-0002 for it.
4. Maybe write a few for important decisions from the past, while people still remember. Don't try to do all of them.

That's all. No big rollout, no process document. Just start with the next decision.

Two years from now, someone will open that folder and understand why your system looks the way it does. Maybe that someone is you.
