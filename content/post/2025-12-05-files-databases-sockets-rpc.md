---
title: "From Shared Files to RPC: How Applications First Talked"
subtitle: "Files, shared databases, sockets and remote procedure calls, scored against what distributed systems need"
description: "File transfer, shared databases, sockets and RPC: how distributed applications first communicated, what each got right, and why decoupling became the goal."
date: 2025-12-05T09:00:00-05:00
author: "Dishant Langayan"
image: "/img/posts/files-databases-sockets-rpc/header.svg"
tags: ["distributed-systems", "integration", "rpc", "architecture", "design-patterns"]
categories: ["eda-design-patterns"]
series: ["Distributed Computing Primer"]
series_total: 5
draft: false
---

> **TL;DR**
> - Shared files and shared databases pass data well, but can't signal anyone. The receiver has to poll, and neither reaches far beyond one network.
> - Sockets fix the trigger and the reach: data arriving on the socket is the signal, and anything with an IP and port is reachable. They also make you write all the plumbing yourself.
> - RPC hides the socket code behind a function call, but the caller still needs to know where the server is, needs it to be up, and waits for it to finish.
> - That gives us three more objectives: decouple from the **location**, the **availability** and the **pace** of other processes. Oddly, files and databases already do much of this. Sockets and RPC do none of it.

<!--more-->

[Last time]({{< ref "2025-11-30-why-distributed-computing.md" >}}) we settled on what distributed computing needs: effective communication between processes. We set three objectives for that communication: it must be able to **trigger** work in another process, **pass data** across, and work at **any scale**.

Now let's look at how applications actually did it, starting with the simplest options.

![Nested layers of the series roadmap with Network Communication highlighted for part 2](/img/posts/files-databases-sockets-rpc/layers.svg)

If you've read *Enterprise Integration Patterns* by Gregor Hohpe and Bobby Woolf (it's on [my reading list]({{< ref "2025-11-25-what-is-event-driven-architecture.md" >}}) for a reason), you'll recognise the order. The book describes [four integration styles](https://www.enterpriseintegrationpatterns.com/patterns/messaging/IntegrationStylesIntro.html): file transfer, shared database, remote procedure invocation and messaging. This post covers the first three, plus raw sockets underneath them. Messaging gets its own post.

## Talking through a file

The simplest way for two processes to communicate: process 1 writes a file to a shared or networked disk, and process 2 reads it.

![File-based and database-centric communication: process 1 writes a file or inserts a row, and process 2 has to keep polling to find out something changed](/img/posts/files-databases-sockets-rpc/files-and-databases.svg)

If you've been around enterprise IT for any length of time, you're probably wincing. It's slow, it's prone to race conditions (is the file finished, or still being written?), and it's everywhere. Plenty of organisations still run critical processes on nightly file drops.

Against our objectives:

- **Pass data:** yes. The file's contents are the data.
- **Trigger:** no. Nothing tells process 2 that a file has arrived. It has to sit there polling the directory, which is busy work, not signalling.
- **Any scale:** no. You're limited to whoever can see the shared disk. To go further you bolt on FTP, SFTP or a managed file transfer product, which literally copies a directory from one host to another so the trick can work across servers.

## Talking through a database

A subtle evolution: swap the file for a database. Process 1 inserts a row, process 2 selects it.

- **Pass data:** yes, as table records, with types and transactions as a bonus.
- **Trigger:** still no. Process 2 is still polling, now with `SELECT ... WHERE processed = false`. Poll too often and you hammer the database. Poll too rarely and you're slow. Teams usually settle on a compromise, which is how a lot of "real-time" systems quietly turned into batch jobs.
- **Any scale:** a little better. The database has an IP address and port instead of a file path, so anyone the network allows can reach it. But a database is a protected resource. You don't open it to the world, and every new consumer adds load and couples itself to your schema.

The real lesson from these two: the only way to communicate at scale across a distributed system is directly over the network. Shared storage is a detour.

## Going over the wire: sockets

So let's talk over the network directly. Two processes, some network between them (Ethernet, Wi-Fi, the internet, it doesn't matter), and a well-known IP address and port where one of them listens. One side writes bytes to the socket and the other side reads them.

![Raw sockets send bytes to a known IP and port; RPC wraps the socket in stubs so a remote call looks local, but the caller still waits on a server that must be up](/img/posts/files-databases-sockets-rpc/sockets-and-rpc.svg)

This is the foundation of the client-server model: clients can be anywhere, as long as the server is somewhere known.

- **Pass data:** yes. Data goes in one end and comes out the other.
- **Trigger:** yes. Data arriving on the socket is itself the trigger for the receiver to act.
- **Any scale:** yes. If there's a reachable IP and port, it can be anywhere. Internet scale is possible.

Three for three. This is the beginning of distributed applications at real scale. But we're not done, because anyone who has written socket code knows the catch.

## Hiding the sockets: remote procedure calls

Raw socket programming is hard. You have to frame messages, serialise data, handle partial reads, reconnect when the connection drops and decide what to do when the other side vanishes mid-conversation. Nobody wants to rewrite that for every interaction.

Remote procedure calls were the answer. You call `placeOrder(order)` as if it were a local function. Generated stubs on each side turn the call into bytes, send them across the network, run the function on the server and send the result back. Sun RPC, CORBA, Java RMI and DCOM all followed this idea, and gRPC is its modern descendant.

RPC ticks the "no socket code" box. But making a remote call look local hides exactly what the fallacies of distributed computing warn about. The caller still needs to know where the server lives. The server still has to be up right now. And the caller still sits and waits until the server is done.

## Let's get greedy: more objectives

Now that we're talking over the network, let's ask for more. First: **shield me from socket programming**. And then the big one, so important I used to say it three times on the slide in my training courses: **decoupling, decoupling, decoupling**.

- **Decouple me from the location of other processes.** Yes, there's an IP and port, but IPs change, ports change and processes move. I don't want that baked into my app.
- **Decouple me from the availability of other processes.** The other side might be down, restarting or cut off by a network blip. That shouldn't be my problem in the moment.
- **Decouple me from the pace of other processes.** If I can produce work faster than the downstream can handle, I don't want to slow down to its speed.

Here's how everything so far scores against all seven objectives:

![Scorecard comparing shared files, shared database, raw sockets and RPC against seven objectives: trigger, pass data, any scale, no socket code, and decoupling from location, availability and pace](/img/posts/files-databases-sockets-rpc/scorecard.svg)

Look at the decoupling columns. Files and databases do surprisingly well. The file sits there whether or not the reader is running, and the reader can take it at its own pace. What they lack is immediacy and reach. Sockets and RPC are the opposite: immediate and far-reaching, but tightly coupled in every way.

What we want is the decoupling of a file drop with the immediacy and reach of a network connection. Put all seven objectives in a basket, give it a good shake, and something specific falls out.

## Wrapping up

Files and databases pass data but can't signal and can't reach far. Sockets signal and reach anywhere, but leave you writing plumbing. RPC hides the plumbing but keeps every kind of coupling. None of them hits all seven objectives.

Next up: what fell out of the basket. Message-oriented middleware, and how putting a broker in the middle delivers all three kinds of decoupling.
