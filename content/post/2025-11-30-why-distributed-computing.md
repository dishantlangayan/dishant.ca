---
title: "Why Distribute? A Primer on Distributed Computing"
subtitle: "What distributed computing is, why we put up with it, and the one thing that makes it work"
description: "What is distributed computing and why do we choose it? Scaling out, fault tolerance and reach, the fallacies that bite, and why it all comes down to communication."
date: 2025-11-30T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/why-distributed-computing/header.svg"
tags: ["distributed-systems", "architecture", "scalability", "microservices", "event-driven-architecture"]
categories: ["eda-design-patterns"]
series: ["Distributed Computing Primer"]
series_total: 5
draft: false
---

> **TL;DR**
> - Distributed computing is many processes, often on many machines in many places, working together so the user sees one service.
> - We accept the extra complexity for three reasons: scaling out is cheaper and goes further than scaling up, failures can be contained and recovered, and services can run closer to their users.
> - The network is not your friend. The classic fallacies of distributed computing (it's reliable, latency is zero, and so on) are still the source of most surprises.
> - Everything rests on one thing: effective communication between processes. We'll judge every communication style in this series against three starting objectives: can it trigger work, can it pass data, and can it work at any scale?

<!--more-->

Picture an application running on one server. One process, one memory space, one database on the same box. A function calls another function and gets an answer in nanoseconds. Nothing gets lost in transit, because there is no transit.

It's a lovely place to be. So why do we keep walking away from it?

This series started life as a talk I gave to customers in training workshops. It builds on earlier work by a colleague at Solace, who came up with the idea that understanding distributed computing is like peeling an onion, one layer at a time. More on that below.

I also recorded a version of this talk for the [Solace Certified Event-Driven Architecture Practitioner](https://solace.com/learn/certifications/event-driven-architecture-practitioner/) certification path. The goal was always the same: before getting to brokers, get everyone on the same page about what they're actually *for*. Event brokers aren't technology for the sake of technology. They exist to make distributed computing work, so that's where we start.

## Peeling back the layers

Think of the series as peeling an onion. The outer layer is distributed computing. Inside it is the question of how distributed processes communicate. Inside that, network communication, then middleware, then asynchronous communication, and right at the centre, event brokers.

![Nested layers showing the series roadmap: Distributed Computing, Distributed Communication, Network Communication, Middleware, Asynchronous Communication and Event Brokers at the centre, with the outer two layers highlighted for part 1](/img/posts/why-distributed-computing/layers.svg)

Each part peels one layer. In this one we cover the outer two: what distributed computing is, why we do it, and what it needs from communication.

## What is distributed computing?

Strip away the textbook language and it comes down to three things:

- **Multiple applications on multiple computers work together** so that they appear to the user as a single service or function.
- **They communicate with each other** to coordinate what they're doing and to share state or pass data along.
- **They may be physically spread out**, across racks, data centres, cloud regions or continents.

![A user sees one checkout service, but behind it six processes run across a Toronto data centre and a Frankfurt cloud region, talking over the network](/img/posts/why-distributed-computing/one-service.svg)

Take an online checkout. To the shopper it's one button. Behind it, a web front end, an order service, payments, inventory, shipping and notifications might all be separate processes, some in your own data centre and some in a cloud region on another continent. The shopper doesn't know or care. If it's done well, it looks like one process, local to them, wherever they are.

## Why go distributed at all?

Every distributed system pays a complexity tax. You choose it on purpose, so it's worth being clear about what you get back.

### Scaling out beats scaling up

When an application on one server runs out of capacity, the easy fix is a bigger server. That's vertical scaling, and it works right up until it doesn't. There's always a largest machine you can buy, and the price climbs steeply long before you reach it.

![Scaling up adds cores to one machine until it hits a hardware or budget ceiling; scaling out adds more commodity machines as demand grows](/img/posts/why-distributed-computing/scale-up-vs-out.svg)

Horizontal scaling adds more ordinary machines instead. It's cheaper per unit of capacity and, in principle, has no ceiling. But you only get to scale out if your application can be split across machines. That's the most common reason teams go distributed.

### Failures you can contain

On one server, when something breaks, everything breaks. Split the work into smaller pieces and you get options. If component B fails, you restart it, or bring up another copy of B somewhere else, and carry on. Operations teams get far more flexibility in how they run the platform, and a fault in one piece doesn't have to take the whole service down.

### Getting closer to the user

Sometimes you have no choice. Your service runs nicely in one location, but your users are spreading across the globe and every round trip across an ocean adds noticeable delay. Or regulation says some data must stay in a particular country. Either way, you end up running parts of the service in more than one place, and those parts still have to agree with each other.

## The catch: the network is not your friend

None of this comes for free. In 1994, L. Peter Deutsch at Sun Microsystems wrote down the assumptions developers wrongly make when they start building distributed systems, and James Gosling later added an eighth. They're known as the [fallacies of distributed computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing):

1. The network is reliable.
2. Latency is zero.
3. Bandwidth is infinite.
4. The network is secure.
5. Topology doesn't change.
6. There is one administrator.
7. Transport cost is zero.
8. The network is homogeneous.

Thirty years on, I still see every one of these in design reviews. "We'll just call the other service" quietly assumes the first three. "We'll hard-code the hostname" assumes the fifth. Going distributed is hard and brings its own pitfalls, so don't do it casually. But sometimes there's no avoiding it, and then the question becomes how to do it well.

## The how: just one bullet point

When I gave this as a talk, the "how" slide had exactly one bullet on it:

**Effective communication between processes.**

That's it. Once you spread a process across machines, the pieces have to talk to each other, reliably and efficiently, and share data as needed. Get the communication right and most of the benefits above follow. Get it wrong and you've built a slower, flakier version of the single server you started with.

So what does "effective" mean? For now, let's set three starting objectives:

- **Trigger:** one process must be able to signal another to wake up and do some work.
- **Pass data:** it must be able to hand over the data or state that work needs, across the process boundary.
- **Any scale:** it should work across a rack, a data centre, an enterprise, or the planet. If we're going to be distributed, why stop at the edge of the building?

We'll hold every communication style in the rest of the series up against these objectives, and add a few more along the way. By the end, you'll have a scorecard that explains why event brokers look the way they do.

## Wrapping up

Distributed computing means many processes acting as one. We pay its complexity tax for scale, resilience and reach, and the network makes us pay it in full. What makes or breaks the whole thing is how those processes communicate.

If you're wondering why any of this matters to the business rather than just to engineers, [Being Event-Driven]({{< ref "2025-11-22-being-event-driven.md" >}}) covers that side.

Next up in the primer: the first ways applications talked to each other, by dropping files on a shared disk and writing rows to a shared database, then going over the wire with sockets and remote procedure calls. Some of them will surprise you by how well they score.
