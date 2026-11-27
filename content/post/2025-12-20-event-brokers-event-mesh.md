---
title: "Event Brokers and the Event Mesh, Explained"
subtitle: "How messaging middleware grew up, and what you get when brokers work together"
description: "What is an event broker, and what is an event mesh? How messaging middleware evolved for hybrid cloud, many protocols, security and governance, and why it matters."
date: 2025-12-20T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/event-brokers-event-mesh/header.svg"
tags: ["event-broker", "event-mesh", "event-driven-architecture", "messaging", "solace"]
categories: ["eda-design-patterns"]
series: ["Distributed Computing Primer"]
series_total: 5
draft: false
---

> **TL;DR**
> - An event broker is the next generation of messaging middleware. It keeps the decoupling we built up in this series and adds what modern environments demand.
> - Those demands: run anywhere from on-prem to cloud to edge, speak many open protocols, enforce security and access control, stay highly available, and support monitoring and governance.
> - The shift in name reflects a shift in thinking: from moving messages between apps to distributing **events**, facts about things that happened, to whoever cares.
> - An event mesh is a set of event brokers connected together, so an event published anywhere reaches every interested consumer, wherever it runs.
> - The two takeaways from the whole series: distributed computing lives or dies on communication, and asynchronous, decoupled communication is what lets it scale.

<!--more-->

We've reached the centre of the onion.

![Nested layers of the series roadmap with Event Brokers highlighted at the centre for part 5](/img/posts/event-brokers-event-mesh/layers.svg)

So far in this series we've decided we need [distributed computing]({{< ref "2025-11-30-why-distributed-computing.md" >}}) for one reason or another, that it lives or dies on communication, and that the network is the only way to communicate at scale. We saw [messaging middleware]({{< ref "2025-12-10-message-oriented-middleware.md" >}}) decouple location, availability and pace, and [why asynchronous communication]({{< ref "2025-12-15-rest-vs-messaging-sync-async.md" >}}) matters more than which protocol you use.

Messaging middleware did all of that well for decades, especially in industries like finance. So why do we now call it an **event broker**? Because we started asking a lot more of it.

## A whole lot more objectives

The environment around middleware changed dramatically. Here's what teams want now, on top of everything in our scorecard.

**Run across any environment.** Your systems might be in your own data centre, in two public clouds, in a factory or on a ship. The communication layer needs to run in all of those places and hide the differences.

**Speak many protocols, including REST.** One proprietary API isn't enough. You may have REST where it fits, JMS in long-lived enterprise apps, AMQP in newer services, and a new business line connecting thousands of sensors or vehicles over MQTT. You want to choose the right protocol for each job and still have them all talk to each other.

**Security and access control.** Once events can flow everywhere, out of their old silos, you need controls over what goes where, for organisational and regulatory reasons. That means encryption, authentication and fine-grained, role-based access.

**High availability and robustness.** If everything flows through this layer, it's a precious resource. The bar was always high, but it's higher when you're no longer inside one safe data centre and parts of your system depend on someone else's cloud.

**Monitoring and governance.** Is the right event reaching the right place at the right time? Are processing SLAs being met? Does an event carrying personal data only go to systems allowed to see it? You need to be able to answer those questions.

**Support event-driven computing.** This is the big one. The industry has moved toward designing systems around events, and the communication layer should support that rather than treat everything as an anonymous message.

![What changed from messaging middleware to event brokers: hybrid and multi-cloud, open protocols, fine-grained security, global scale, governance, and mediating event-driven apps](/img/posts/event-brokers-event-mesh/then-and-now.svg)

Put all of that into the requirements basket, shake it again, and what falls out is the next stage in the evolution of messaging middleware.

## From messages to events

Anyone who has worked with message queues will look at a modern event broker and say "that's just MQ". There's some truth in that. The lineage is direct. But the name change reflects a real change in how we think.

A message is a generic container: "here's some data, deliver it". An **event** is a notification that something happened: an order was placed, a flight landed, a sensor crossed a threshold. The producer doesn't know or care who's interested. It simply states a fact, and the broker makes sure everyone who subscribed hears about it. That's the mindset shift I wrote about in [Being Event-Driven]({{< ref "2025-11-22-being-event-driven.md" >}}), now showing up in the technology.

(If you want a sharper vocabulary for the different ways people use "event", Martin Fowler's [What do you mean by "Event-Driven"?](https://martinfowler.com/articles/201701-event-driven.html) is still the best short read.)

Event brokers come in different shapes. Some are built around queues and topics with rich routing, some around an append-only log you can replay, and many now do a bit of both. Apache Kafka, RabbitMQ, Apache Pulsar, Solace Event Broker and cloud services such as Amazon EventBridge, Azure Event Grid and Google Cloud Pub/Sub all live somewhere in this space. Which one fits depends on your use case far more than on any feature checklist.

## The event mesh

One broker in one data centre doesn't meet the "any environment" objective. That's where the **event mesh** comes in: event brokers deployed wherever your applications are, connected together so they behave as one.

![Four event brokers in an on-prem data centre, two cloud regions and a factory edge, linked into a mesh; apps attach using JMS, REST, AMQP, Kafka, MQTT and WebSocket](/img/posts/event-brokers-event-mesh/event-mesh.svg)

An application publishes an event to its local broker, using whatever protocol suits it. The mesh works out which consumers anywhere are interested and routes the event to them, only crossing a wide-area link when someone on the other side has actually subscribed. Producers and consumers stay decoupled in location, availability and pace, now at global scale.

Put another way, an event mesh is a dynamic infrastructure of interconnected event brokers that moves events across different environments, translates between protocols, applies security and governance, and stays highly available along the way.

If you know the synchronous microservices world, there's a useful parallel. A **service mesh** helps services find and call each other reliably with request/reply. An **event mesh** does the equivalent job for asynchronous, event-driven communication.

Full disclosure: I work at Solace, and the event mesh is an idea we've championed for years. If you want the vendor take, Solace's [What is an event mesh?](https://solace.com/what-is-an-event-mesh/) goes deeper. But the concept doesn't depend on any one product, and open specifications like [AsyncAPI](https://www.asyncapi.com/) for describing event-driven APIs and [CloudEvents](https://cloudevents.io/) for describing event data help keep it that way.

## Back to the onion

Peeling it one last time, from the outside in:

1. We go **distributed** for scale, resilience and reach.
2. That means processes must **communicate**, and the only way to do that at scale is **over the network**.
3. **Middleware** spares us the socket code and decouples location, availability and pace.
4. That decoupling comes from **asynchronous** communication, not from any one protocol.
5. **Event brokers**, connected into an **event mesh**, carry it across every environment we now run in.

If you remember just two things from this series, make them these. First, distributed computing is only as good as the communication between its parts. Second, asynchronous, decoupled communication is what lets it scale.

## Wrapping up

That's the Distributed Computing Primer. We started with one happy server and ended with brokers spread across clouds and factories, and every step was driven by the same few objectives.

Next on the blog is the EDA Design Patterns series, where we put all of this to work: how to be distributed, how to be scalable, and how to handle data well when everything is asynchronous. Remember those duplicates and out-of-order messages from part 3? They're coming back.
