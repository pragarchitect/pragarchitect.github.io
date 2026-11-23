---
layout: post
title: Nobody reads the 80-page architecture document
categories: [architecture, documentation]
---

<!-- Marcel: example situation, replace with your own if you like. -->
At one of my previous assignments I found it on the SharePoint. The Software Architecture Document. 80 pages, version 2.3, last changed three years ago. It had a nice table of contents and a lot of diagrams in a style I recognised from Visio.

I asked the team who had read it. Nobody. Not even the parts about their own system.

I don't blame them. And I don't blame the architect who wrote it either. Somebody asked for a document, so a document was made. That is how it often goes.

{% include pullquote.html quote="Documentation that nobody reads is not documentation. It is an archive." %}

The problem with big documents is not that they are big. It is that they are written once, at the start, when you know the least. After that the system changes every sprint and the document doesn't. After a few months nobody trusts it anymore. And once people don't trust it, they stop reading it, so nobody notices it is wrong. A nice vicious circle.

What works better in my experience:

- **Small pieces.** One decision per ADR, one diagram per audience. Small things are easy to read and easy to update.
- **Close to the code.** Put it in the repository. If it lives next to the code, it gets changed together with the code.
- **Written when needed.** Not everything up front. Write it down on the moment the team needs it, or on the moment a decision is made.
- **Easy to find.** If people have to search for it, they will ask a colleague instead. Which is fine, but then you didn't need the document.

Is there never a place for a bigger document? Sometimes there is. A tender, an audit, a handover to another supplier. But then generate it from the small pieces, if you can. Don't make it the place where the truth lives.

Write less. Keep it close. Keep it true. That is already a lot.
