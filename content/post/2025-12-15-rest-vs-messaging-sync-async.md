---
title: "What About REST? Synchronous vs Asynchronous"
subtitle: "Running the same system over HTTP, and the distinction that actually matters"
description: "REST vs messaging for distributed systems: how HTTP scores on decoupling, why microservices don't have to mean REST, and when to choose sync or async."
date: 2025-12-15T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/rest-vs-messaging-sync-async/header.svg"
tags: ["rest", "microservices", "messaging", "distributed-systems", "event-driven-architecture"]
categories: ["eda-design-patterns"]
series: ["Distributed Computing Primer"]
series_total: 5
draft: false
---

> **TL;DR**
> - HTTP/REST is the easiest way to talk over a network: every language, tool and developer already speaks it. It fully hides socket programming.
> - It doesn't decouple you from location, availability or pace. You need an address, the service has to be up, and a chain of calls is only as fast as its slowest link.
> - You can claw some of that back with DNS, load balancers, service discovery, retries and circuit breakers, but it's work you're adding to make up for the style.
> - The real dividing line isn't REST vs messaging. It's **synchronous** (call and wait) vs **asynchronous** (send and get told later).
> - Microservices don't have to mean REST. Use request/reply where someone is waiting for an answer, and asynchronous messaging for everything else.

<!--more-->

[Last time]({{< ref "2025-12-10-message-oriented-middleware.md" >}}), message-oriented middleware finally went green across our whole scorecard of objectives: trigger, pass data, any scale, no socket code, and decoupling from the location, availability and pace of other processes.

By this point there's always someone thinking: "That's nice, but I already do network communication. I use HTTP. Why would I need a broker?"

It's a fair question, and the answer is worth getting right, because a lot of teams skip straight from files and databases to REST without ever meeting messaging.

![Nested layers of the series roadmap with Asynchronous Communication highlighted for part 4](/img/posts/rest-vs-messaging-sync-async/layers.svg)

## The same store, over HTTP

Let's take the online store from part 3 and rebuild it with HTTP. The order processor and cancellation processor each run an HTTP listener. The web front end makes a `POST /orders` call for a new order and a `POST /orders/{id}/cancel` for a cancellation.

![The web front end calls POST /orders on the order service and POST /orders/{id}/cancel on the cancellation service, each at a known host and port](/img/posts/rest-vs-messaging-sync-async/rest-flow.svg)

Simple, readable, and something every developer on the team already knows how to build.

## How HTTP scores

**No socket code: yes, and then some.** This is HTTP's superpower. Every language has an HTTP client in its standard library. Browsers, proxies, API gateways, testing tools and observability all speak it. There's very little plumbing to do.

**Location decoupling: partly.** HTTP is built on a URL, so somewhere there's a hostname and port that the caller has to know. DNS and load balancers help. In larger systems teams add service discovery, or a service mesh, so apps look up where things are at runtime instead of hard-coding them. It works, but it's extra infrastructure you're adding to work around the style.

**Availability decoupling: only if you build it.** If the order service is down when the front end calls, the call fails. Now the front end (or a load balancer in front of it) needs retry logic, timeouts, a circuit breaker, maybe a fallback, and something to do with the order while it waits. With guaranteed messaging, the broker held on to the order for you.

**Pace decoupling: no.** This is the one that hurts most. When one request fans out into a chain of calls, the caller waits for all of them.

![Synchronous: the front end calls Orders, which calls Inventory, which calls Payments, so response time adds up to the slowest link. Asynchronous: the front end publishes to a broker and the consumers work in parallel](/img/posts/rest-vs-messaging-sync-async/sync-vs-async.svg)

The user feels the sum of every hop, and the whole flow runs at the speed of its slowest service. Worse, when one service slows down, callers pile up waiting on it, their threads and connections run out, and the slowdown spreads upstream. These cascading failures are notoriously hard to diagnose, because the service that looks broken usually isn't the one that is.

Here's the updated scorecard:

![Scorecard comparing shared files, shared database, raw sockets, RPC, messaging middleware and HTTP/REST against seven objectives; HTTP is partial on location and availability and fails on pace](/img/posts/rest-vs-messaging-sync-async/scorecard.svg)

If the HTTP row looks a lot like the RPC row from [part 2]({{< ref "2025-12-05-files-databases-sockets-rpc.md" >}}), that's not a coincidence. A REST call is, in practice, a very well-tooled remote procedure call.

## The real distinction: sync vs async

So the difference isn't really REST versus brokers. It's **synchronous versus asynchronous** communication.

**Synchronous** means you make a call, wait for it to finish, then decide what to do next. It's like ordering at a coffee counter and standing there until your drink is ready. Simple, but you can't do anything else, and the line behind you grows.

**Asynchronous** means you send the work off and carry on. You'll be told when it's done. It's the restaurant buzzer: you order, sit down, talk to your friends, and come back when it buzzes. Meanwhile the kitchen works through orders at its own pace, and can bring in another cook when it gets busy.

Asynchronous is harder to think about. You have to handle "not done yet", design for things finishing out of order, and accept that different parts of the system will catch up at different times. But it lets you parallelise work, absorb spikes and survive failures, which is exactly what you need to scale.

## Microservices don't have to mean REST

There's a popular belief that microservices and REST go hand in hand. Plenty of developers learn microservices from tutorials where every service exposes a REST API and calls the others over HTTP, and never encounter messaging at all.

The result is often a "distributed monolith": dozens of small services, all calling each other synchronously, all needing each other to be up at the same moment. You get the operational complexity of distribution without the decoupling that was supposed to pay for it.

## So when should you use which?

HTTP has its place, and it's a big one. I'm not saying replace it. A rough guide:

**Reach for request/reply (often REST) when:**
- someone is actively waiting for the answer, such as a user loading a page or a query for current data,
- you're exposing a public or partner API where HTTP is the expected contract,
- the interaction is a simple read or update with no downstream chain.

**Reach for asynchronous messaging when:**
- the work takes time, or the caller doesn't need the result right now,
- one thing happening should trigger several others (fan-out),
- traffic is spiky, or consumers are slower than producers,
- parts of the system live in different places or fail independently.

And they mix well. A common approach is to accept a request over HTTP, return `202 Accepted` straight away, hand the work to a broker, and let the client check back or be notified later. Microsoft documents this as the [Asynchronous Request-Reply pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/async-request-reply). Many modern brokers also accept REST directly, so an HTTP-only client can publish into an asynchronous system.

## Wrapping up

REST is the friendliest way to talk across a network, and it deserves its popularity. But on its own it's synchronous, so it inherits the coupling we've been trying to get rid of since part 2. Asynchronous messaging is what decouples location, availability and pace. Most real systems need both, used deliberately.

Next up, the final layer: how messaging middleware grew into event brokers, and how brokers connect into an event mesh that spans data centres, clouds and the edge.
