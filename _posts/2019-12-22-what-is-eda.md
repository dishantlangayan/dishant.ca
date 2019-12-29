---
layout: post
current: post
cover:  assets/images/building-architecture.jpg
navigation: True
title: What is Event Driven Architectures?
date: 2019-12-22 10:00:00
tags: [Event Driven Architectures]
class: post-template
subclass: 'post tag-eda'
author: dishant
---

As this blog is mainly about Event Driven Architectures (EDA), I thought of starting off with the basics and explaining some fundamentals.

Everything in the Universe is event driven. The big bang was an event. An inventory update is an event. A traffic light update is an event. Internet of Things (IoT) is inherently event driven. My washing machine notifying me on my phone that my laundry is done is event driven. Life can be perceived as a series of events, which we "subscribe" to by given them attention. 

> An architectural pattern that promotes the production, consumption, detection, and reaction to such events in real-time is refered to as an **Event Driven Architecture (EDA)**. 

EDAs have existed for years, handling high-volumes of events in banking systems, stock trading, travel reservation systems to name a few. It has allowed exterprise systems to easily extend their architectures with new components that are ready to produce new events or consume those that already are in place in the overall system. It decouples systems and allows them to react to events in real-time. 

## Benefits of EDA

Embracing EDA is foundational to building next generation of digital business applications. IT teams will need to be able to design, develop, deploy and operate event driven solutions, in cloud native environments. In doing so, EDA will help bring one of the most important business value to enterprises: **Customer Experience**.

![Business Value of EDA](/assets/images/business-value-eda.png)

Every business strive to deliver better customer experiences to their consumers. To deliver on these experiences, enterprises need to their businesses and IT systems to be event driven, implement EDAs, and reacts to events in real-time. Being event driven means more real-time which = **efficiency**. Event driven means **liberating your data**, i.e. breaking down silo’s of data and making this data available to other lines of businesses withing the organization. Event driven also means decoupling of microservices/applications, which provides better **agility**. All of this lands back to customer experience.

Now that you have an idea of what EDA means let's explore some other related concepts.

## Events

In the beginning of the article I attempted to explain what events are, but in technical terms, an event is a way of capturing a statement of fact. Other similar definitions on the web explain an event as:

* Events represent the change of state of something of interest to the business.
* Events are records of something that has happened.
* Events can't be changed, that is, they are immutable. (We can't change something that has happened in the past).

## Messaging

I previously mentioned EDAs have been around for some time. These architectural patterns were more commonly referred to as Enterprise Integration Patterns (EIP). With EIPs you could build multiple applications independently, with different languages and platforms, that share data with each other in a responsive way. You would use Messaging to transfer packets of data frequently, immediately, reliably, and asynchronously, using customizable formats. Messaging Systems or a.k.a *Message Brokers*, would act as the intermediary broker between these decoupled applications. The decoupling allows integrators to choose between broadcasting a message to multiple receivers (a.k.a publish-subscribe), routing a message to one of many receivers, or other topologies like point-to-point and request-reply.

![Messaging](/assets/images/messaging.gif)

In EDA, we refer to these messaging systems as Event Brokers.

A sender sends data to a receiver by sending a *Message* via a *Message Channel* that connects the sender and receiver.

![Messaging Channel](/assets/images/message-channel-solution.gif)

A channels are logical addresses in the messaging systems. The two most common type so channels used in messaging systems are Topics and Queues. I cover these in more details in subsequent articles, but for now all you need to know is that every message is destined to one address, and other applications subscribe to the destination to receive the message.

## Events vs. Messages

To be added soon.

## Event Brokers

To be added soon.

## Event Schemas and Registry

To be added soon.

---
Cover photo by [Lance Anderson](https://unsplash.com/@lanceanderson?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on Unsplash.
Messaging photos by [www.eaipatterns.com](https://www.eaipatterns.com)