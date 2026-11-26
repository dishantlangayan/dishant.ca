---
title: "What is Event-Driven Architecture? My Reading List"
subtitle: "Six resources that helped me understand EDA, and why each one is worth your time"
description: "What is event-driven architecture? Rather than another explainer, here are the guides, course, certification and books that helped me learn EDA."
date: 2025-11-25T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/what-is-event-driven-architecture/header.svg"
tags: ["event-driven-architecture", "eda", "messaging", "design-patterns", "integration", "learning-resources"]
categories: ["eda-design-patterns"]
draft: false
---

> **TL;DR**
> - You don't need another "what is event-driven architecture" explainer. Plenty of good ones exist, so this post points you to the ones I keep going back to.
> - Start with a broad guide (Solace's or Microsoft's) to get the vocabulary: producers, consumers, brokers, topics, pub/sub vs streaming.
> - Go deeper with a structured course or certification if you learn better with a path and a finish line.
> - Read *Event-Driven Architecture in Golang* to see the patterns in working code, and *Enterprise Integration Patterns* for the messaging foundations underneath it all.

<!--more-->

When I started digging into event-driven architecture back in 2017, there wasn't much on the internet to read and learn from. My first encounter with the ideas behind it was a book I had bought in 2012 called *Enterprise Integration Patterns*. Although it wasn't about EDA, it laid the foundations for how I design and build event-driven solutions on messaging platforms. It wasn't until between 2017 and 2020 that the terms "EDA", "real-time" and "event-driven systems" started to pick up. Now there are numerous blog posts, vendor pages, videos and courses you can find online. In fact, I created a certification course on event-driven foundations myself, back when there were very few resources to learn from. A lot of what's out there says the same thing in slightly different words, and some of it contradicts the rest.

So I'm not going to write yet another "What is EDA?" explainer. The good ones already exist. Instead, here's the short list of resources that actually moved my understanding forward, with a line on what each one covers and why I'd recommend it.

If you haven't read my previous post, [Being Event-Driven]({{< ref "2025-11-22-being-event-driven.md" >}}), it covers the business side of the story. This list is for the technical side.

## Start here: the guides

These two are free, readable in one sitting, and give you the vocabulary you'll need for everything else.

**1. [The Complete Guide to Event-Driven Architecture](https://solace.com/what-is-event-driven-architecture/#what) (Solace)**
A broad introduction to what EDA is, what counts as an event, why pushing events beats polling, and the core concepts like event brokers, topics, event mesh, eventual consistency, choreography and CQRS.

Full disclosure, I work at Solace. I'd still recommend it because it covers a lot of ground without assuming you've already picked a technology, and the real customer examples help make it concrete.

**2. [Event-Driven Architecture Style](https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/event-driven) (Microsoft Azure Architecture Center)**
A concise, practical reference on producers, channels and consumers, pub/sub vs event streaming, payload design, and a list of when *not* to use EDA and the challenges you'll hit (ordering, eventual consistency, error handling, schema evolution).

I don't agree with some of the points in the "when not to use it" section, though. There are ways to solve eventual consistency, and just because a team doesn't have experience operating distributed asynchronous systems doesn't mean the business shouldn't adopt EDA. In the majority of cases, the benefits of EDA outweigh the cost of training a team to operate event-driven systems.

## Go deeper: a course and a certification

If you're the kind of person who learns better with a structured path and some way of knowing you've finished, one of these will suit you.

**3. [Event Driven Architecture: The Complete Guide](https://www.udemy.com/course/event-driven-architecture-the-complete-guide) (Udemy, Memi Lavi)**
A roughly three-hour video course covering EDA fundamentals, orchestration vs choreography, event sourcing and CQRS, sagas, logging with correlation IDs, and a small hands-on build with RabbitMQ.

It's a good fit if you want the architect's view in a weekend rather than a month. Udemy courses go on sale constantly, so don't pay full price.

**4. [Solace Certified Event-Driven Architecture Practitioner](https://solace.com/learn/certifications/event-driven-architecture-practitioner/) (Solace Academy)**
A free, self-paced learning path and exam covering the motivations for EDA, topic taxonomy design, event-driven design patterns and microservices, and how to take a first project from pilot to quick win.

This is the certification path I developed in 2020, when only a few online resources offered a structured way to learn EDA. That was before Microsoft, AWS and Google had any courses or certifications on EDA. It has been refreshed a few times and is still relevant today, and at the time of writing over 2,000 learners have completed the certification. Most of the course focuses on the foundational concepts and the approach to take when building event-driven solutions. So if you don't end up reading any of the other resources, this is the one I'd recommend. Plus, you'll get to add another credential to your certification list.

## Read the books

Guides and courses give you the shape of things.

**5. [Event-Driven Architecture in Golang](https://play.google.com/store/books/details?id=ovKZEAAAQBAJ) (Michael Stack, Packt, 2022)**
A hands-on guide to building asynchronous, event-driven microservices in Go, covering patterns for data consistency and resiliency, event sourcing, CQRS and domain-driven design, with choreography and orchestration shown in a working sample application.

If you learn best by seeing the patterns in real code, this is the one to keep on your desk. The examples are in Go, but the patterns carry over to whatever language you use.

## An all-time favourite: Enterprise Integration Patterns

**6. [Enterprise Integration Patterns](https://martinfowler.com/books/eip.html) (Gregor Hohpe and Bobby Woolf, Addison-Wesley, 2003)**
The classic catalogue of 65 messaging patterns: channels, routers, translators, endpoints and the rest of the vocabulary for connecting systems asynchronously.

This one isn't an EDA book, and it's older than most of the technology we use today. It's still one of my all-time favourites. It's where I really learned how messaging and integration fit together, and many of its patterns, like publish-subscribe channels, message routers, dead letter channels, idempotent receivers and competing consumers, are the foundations modern event-driven systems are built on. You'll find them under different names in every broker's documentation.

Don't let the 2003 publication date put you off. The products in its examples have aged, but the patterns haven't. Gregor keeps a companion site at [enterpriseintegrationpatterns.com](https://www.enterpriseintegrationpatterns.com/) with the pattern catalogue and icons, which is a handy reference once you've read the book.

Gregor also published a blog post in 2024, [Event-driven = Loosely coupled? Not so fast!](https://www.enterpriseintegrationpatterns.com/ramblings/eventdriven_coupling.html), where he looks at events and coupling and relates them back to the patterns covered in the book.

## How I'd use this list

If you're new to EDA, read the two guides first, then pick either the course or the certification depending on how you like to learn. Once the concepts click, move on to the Golang book to see them in working code.

Keep *Enterprise Integration Patterns* nearby throughout. Whenever something in a newer resource feels like it's been named three different ways, there's a good chance Hohpe and Woolf named it first.

## Wrapping up

There's no shortage of content on event-driven architecture. The trick is picking a few good sources and going deep instead of skimming everything. These six have held up for me, and I still go back to most of them.

Next up is the Distributed Computing Primer, a five-part series on how systems learned to talk to each other, from shared files and RPC through message brokers and REST, to the event brokers and event meshes we use today.

Got a resource that should be on this list? Let me know.
