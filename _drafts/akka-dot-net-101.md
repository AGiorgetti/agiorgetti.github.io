---
layout: post
title: Akka.Net 101 - an Open Source introductory course to Akka.Net
comments: true
disqus_identifier: 27043952-0ba1-44e7-b001-df4273b68e38
tags: [akka.net,actor model,actor framework,.net core]
---

In the recent years I had a chance to work with highly concurrent systems using several development practices and patterns, like DDD, Event Sourcing, CQRS and the Actor Model, just to name a few.

I tested and worked with some Actor Frameworks in the .NET world: [Akk.Net](https://getakka.net/) and [Proto.Actor](http://proto.actor/).

I used to have a private project with the source code of an Akka.Net Introductory Course I've elaborated over the months.

With the occasion of the "Un Actor (Model) per amico - Multithreading made easy" [DevMarche](http://dev.marche.it/) User Group event we held in the last week of February, I've decided to "reshape" the course source code and open source it.

So here it is: a 4 Units Introductory Course that will guide you through most of the features Akka.Net has.

[Akka.Net 101](https://github.com/PrimordialCode/akkadotnet101)

You can take on all the lessons following the tutorials, from the starting point to the completion of each unit.

During the course we'll implement a simple application that can be used to test the features of the framework, showing what happens when we create, start and stop actors, when an exception is raised inside an actor and the supervision strategy kicks in.

We'll also take advange of the routing mechanics to distribute the workload amongst multiple working actors; we'll see how we can have remote process communicate (using Akka.Remote) and configure a simple cluster of nodes with Akka.Cluster.

The idea is to have an Open Source Couse that will possibly evolve over time, maybe taking advantage of your contributions.

cya next!
