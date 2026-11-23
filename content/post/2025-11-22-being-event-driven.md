---
title: "Being Event-Driven: A Business Mindset First"
subtitle: "Why event-driven is as much about how your business works as how your code works"
description: "Being event-driven isn't just a technology choice. Learn what an event-driven business is, how real-time events create value, and the goals to aim for."
date: 2025-11-22T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/being-event-driven/header.jpg"
tags: ["event-driven-architecture", "eda", "architecture", "digital-transformation", "event-broker"]
categories: ["eda-design-patterns"]
draft: false
---

> **TL;DR**
> - Being event-driven has two halves: the technology you build with, and the way your business describes and organizes what it does. Most teams only work on the first.
> - An event is simply something that happens and matters. Every business can list the events that matter to it without writing a line of code.
> - The value is in timing. Acting on an event while it's still happening lets you upsell, inform, reward and react in ways that a nightly batch or next-day email can't.
> - An event-driven business is always sensing, always ready and always learning, which is what digital transformation is really asking for.
> - Set your technology goals as agility, efficiency and liberated data, all in service of the best possible customer experience.

<!--more-->

Here's a quick quiz. How long has "event-driven" been around as an idea?

If you guessed "a couple of years, it's the new hype", you'd be in good company, but the idea has been floating around since at least the mid-2000s. And if you've ever built a GUI, you've written event-driven code: a button gets clicked, a handler runs. Nothing new there.

So why does being event-driven feel like a fresh conversation again? Because the term has grown up. It no longer just describes how you write code. It describes how a business senses and responds to the world around it, and that's the part most of us technologists skip.

## Technologists, it's time for a reset

If you still think being event-driven is about the language, framework or broker you pick, I'd ask you to hit the reset button with me.

Event-driven has two halves. One half is technology: event brokers, pub/sub, streams, asynchronous services. The other half is business: how you describe your processes and how you organize your capabilities around the things that happen to your business.

![Two halves of being event-driven: a technology half (brokers, pub/sub, async services) and a business half (business events, processes, capabilities organized around events)](/img/posts/being-event-driven/two-halves.svg)

If you only take one thing away from this post, make it this picture. The technology half is necessary, but on its own it gives you a nicely decoupled system that nobody in the business knows how to get value from. The business half is where the opportunities show up.

## So what is an event-driven business?

First, pause the technical side of your brain. It's harder than it sounds. Resist the urge to think about topics, schemas or delivery guarantees for the next few minutes.

One of the best things about the word "event" is that it isn't a technical term. Look it up in a dictionary and you'll get something like: *a thing that happens, especially one of importance*. That's it. Something happened, and it matters.

Which means any business can sit down and list the events that matter to it. A few examples:

- **An airline:** a passenger arrives at the check-in desk. A flight lands and the passenger reaches their destination.
- **A connected car maker:** the fuel level drops low. The odometer passes a service threshold.
- **A retailer:** a stock delivery arrives at the store. The quantity of a product on the shelf drops below a level.

None of those needed a whiteboard full of boxes and arrows. They're just things that happen in the course of doing business. Each one carries some value, if you're able to act on it in time.

## The value is in the timing

Once you know your events, the next question is what you can do when they happen. Let's stay with the airline.

![An airline passenger journey with three moments: at check-in an upgrade offer, at security a gate and walk-time notification, after landing a new status tier reward](/img/posts/being-event-driven/airline-moments.svg)

**Upsell.** The passenger is about to check in. A process looks at how full the flight is, sees that the next cabin is nearly empty, and offers a discounted upgrade. Timing is everything here. Offer it a week early and the price may not appeal, or they'll put off the decision and forget. Offer it after boarding and it's worthless. The data behind the offer has to be live too: the price only works if it reflects how many seats are actually free right now.

**Inform.** Another process checks the gate status and tells the passenger which gate they're at and roughly how long the walk is. Anyone who has refreshed the departures board every two minutes knows how much stress that removes. Calmer customers are customers who fly with you again.

**Reward.** The flight lands and the passenger has just crossed into a new loyalty tier. You could tell them in an email three days later, when the trip is a fading memory. Or you can tell them as they step off the plane, and attach that little burst of satisfaction to that flight with your airline.

Same events, but acting in the moment turns them into a better chance of a sale, information people can actually use, and loyalty that feels personal.

The other examples follow the same pattern. For the car maker, a low-fuel event becomes the trigger for a new product: a service that comes to your home or office and fills the tank. A service-due event stops being an anxiety-inducing warning light and becomes "there's an appointment available tomorrow, want it?" The manufacturer becomes part of the owner's ongoing journey instead of a one-time sale.

For the retailer, a stock-arrival event becomes a "the item you reserved is in" notification. The more interesting case is reacting to what's happening on the ground. Think back to early 2020 and the great toilet paper shortage. In Canada, retailers eventually put per-customer limits in place, but by then shelves were empty and customers were already frustrated. If sales events had been watched as they happened, the buying spike could have been spotted early and purchases throttled before a small group cleared the shelves for everyone else. Being event-driven is how a retailer stays present and responsive instead of finding out from the weekly report.

## Always sensing, always ready, always learning

Gartner has been making this argument for years, stating plainly that ["event-centric processing is the native architecture for digital business"](https://www.gartner.com/en/newsroom/press-releases/2017-07-10-gartner-says-cios-must-define-an-event-centric-digital-business-strategy/). The way I've always summarized their view of an event-driven business is that it is:

- **always sensing** what's happening, inside and outside the company,
- **always ready** to respond while it still matters,
- **always learning** from what happened so the next response is better.

Notice the phrase "digital business". Most companies aren't digital natives. They're banks, insurers, hospitals, retailers, telcos, even law firms that have operated the same way for decades or centuries. Becoming sensing, ready and learning is exactly what they're trying to do, and we call that effort digital transformation.

## What digital transformation actually means

The term means something slightly different in every industry, so it helps to agree on a definition. My favourite is from [The Enterprisers Project](https://enterprisersproject.com/what-is-digital-transformation):

> "Digital transformation is the integration of digital technology into all areas of a business, fundamentally changing how you operate and deliver value to customers. It's also a cultural change that requires organizations to continually challenge the status quo, experiment, and get comfortable with failure."

Three parts of that stand out to me. *All areas of a business*: technology is the competitive advantage, not a cost centre. *Fundamentally changing how you operate*: so the way you operate becomes technology-led. And *challenge the status quo*: what got you here won't get you there, and you need to be comfortable failing fast while you figure out what will.

## Pivoting back to technology

OK, you can switch your technical brain back on. The technologies driving this shift will be familiar: public cloud that lets you deploy almost anywhere on demand, AI and machine learning (and now AI agents) that are far more accessible than they were a few years ago, microservices and containers that let you build and scale small pieces independently, integration platforms that connect systems you didn't build, and API gateways that let partners and suppliers plug in safely.

Every one of those produces events or needs to consume them. Something has to get the right events from where they happen to everywhere they're needed, across clouds, data centres and partners. That's the job of the event broker in the middle, and when you connect brokers together, an event mesh. (Full disclosure: I work at Solace, which builds exactly this, but the idea stands regardless of which broker you choose. We'll dig into brokers and meshes properly in the Distributed Computing Primer series.)

## Technology goals for becoming event-driven

So this isn't technology for technology's sake. When you start the journey, set goals that tie back to the business half.

![Technology goals for becoming event-driven: agility, efficiency and liberation surrounding the central goal of delivering the best customer experience](/img/posts/being-event-driven/goals.svg)

- **Agility:** make systems more flexible and scalable so you can ship new ideas, like that upgrade offer, faster.
- **Efficiency:** get the right event to the right application at the right time, without polling or batch jobs.
- **Liberation:** break down silos so events and data can flow to any part of the business that can use them.

And at the centre of all three: **deliver the best customer experience you can.** Every example in this post, from the gate notification to the reserved item, comes back to that.

## Wrapping up

Being event-driven starts with a mindset, not a product. Name the events that matter to your business, think about what you could do if you acted on them in the moment, and then pick technology that helps you sense, respond and learn.

Next up, I'll share the reading list that helped me most when I was getting my head around event-driven architecture, followed by the Distributed Computing Primer, a five-part series on how systems learned to talk to each other and how we ended up with event brokers in the middle.

---

*Cover photo by [Steve A Johnson](https://unsplash.com/@steve_j) on [Unsplash](https://unsplash.com/photos/a-computer-circuit-board-with-a-brain-on-it-_0iV9LmPDn0).*
