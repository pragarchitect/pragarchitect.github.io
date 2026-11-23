---
layout: post
title: Your architecture repo is the best prompt you have
categories: [architecture, AI]
---

I have been talking about AI and software architecture for a few years now, at conferences like TEQNation and aiGrunn. In the beginning my talks were mostly about using ChatGPT as a sparring partner. Ask it for options, let it find holes in your design, generate a first version of an ADR.

That still works. But I noticed something along the way. The quality of what you get back depends almost completely on the context you give. Ask a general question, get a general answer. Give it your actual system, your decisions, your constraints, and suddenly the answers become useful.

And then I realised: the teams that already did architecture as code had a big advantage. They didn't need to write long prompts. The context was already there.

{% include pullquote.html quote="Architecture as code was meant for humans. It turns out machines like it too." %}

## Why plain text matters

Think about what an AI agent can do with each of these:

- **ADRs in Markdown.** The agent can read every decision and the reason behind it. It can check if a new change is in line with earlier decisions, or point out that you are about to undo something on purpose.
- **C4 models in Structurizr DSL.** The structure of the system, in plain text. The agent knows which containers exist, how they talk to each other and who owns what. It can also update the model when you add something.
- **ArchUnit tests.** Your architecture rules, as code. The agent can read them to understand the boundaries, and the tests will catch it when it breaks them.

Now compare that with architecture in a Visio file on SharePoint, or in a PowerPoint from 2021. The agent can't see it. And even if you paste it in, it is probably outdated.

## What this looks like in practice

Some things I see working, or that I have tried myself:

- **Generating a draft ADR from a pull request.** The agent looks at the code change and the existing ADRs, and writes a first version. A human still needs to fix the "why", because that is often not in the code. But the structure and the options are a good start.
- **Asking architecture questions with context.** "Which services would be affected if we split the order database?" With the C4 model in the repository, the agent can give a real answer instead of a generic one.
- **Checking consistency.** "Is there any ADR that conflicts with this change?" Hard to do by hand when you have fifty ADRs. Easy for an agent.
- **Keeping diagrams in sync.** When a new service is added, let the agent propose the update to the Structurizr model in the same pull request.

## A word of caution

AI makes mistakes, and it makes them with a lot of confidence. It can invent a decision that doesn't exist, or miss a constraint that is obvious to the team. So treat it as a junior colleague with a very good memory. Useful, fast, and in need of review.

Also, the agent only knows what is written down. If the important decisions live in the head of one senior developer, the agent doesn't know them either. Which is, in a way, the same problem we had before AI.

## The nice side effect

Here is what I like most about this. For years I tried to convince teams to write ADRs and keep their diagrams up to date. The argument was always "it helps your future colleagues". A good argument, but the future is far away.

Now there is a reason that pays off today. Write it down, and your AI tools get better immediately. That turns out to be quite motivating.

Good architecture documentation is good context. For humans and for machines.
