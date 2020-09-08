+++
title = "Minimum Viable Architecture (MVA)"
date = "2020-09-07"
description = "MVA is a way to describe the minimum required architecture and infrastructure to create an MVP"
tags = [
    "mva", "mvp", "minimum viable architecture",
]
author = "Juan Carrey"
+++

The minimum viable architecture was firstly brought to me in a conversation with 
[Drew Burlingame](https://github.com/drewburlingame) which was our Senior Architect back then.

I hope this post comes handy for some of those starting greenfield projects,
 even personal pet projects of their own and have emotions of hype mixed with pragmatic engineering.

The concept is pretty simple, even though it is applicable to any design or solution, it might be easier to explain it
alongside with the very well known Minimum Viable Product, as these should come
together at any proof of concept or starting project with limited resources.

## What is Minimum Viable Architecture or MVA ?

MVA is a version of the architecture with just enough features to satisfy MVP requirements 
and reducing the technical debt accrued during going live for the first version

In essence, is trying to simplify as much as we can the initial architecture of the product,
minimizing the risk and costs of refactoring to meet the "goal" architecture.

## Pragmantic Examples

Mosts of the times, when a new greenfield product starts, engineers tend to think big,
overengineering the architecture design, adding all sorts of cloud services provided, like they are 
just a pick and go.

### Microservices vs Monolith

I do not think an MVP would ever need a microservices architecture, in fact, I would expect a minimum
devops needs for an MVP. A simple CI&CD now that we have lot's of free tiers out there to run simple build+deploy tasks
is more than enough.

Start monolith, then start splitting once the concept has succeeded and you are ready for it.

### Async Bus vs InMemory Mediator

Alongside with microservices, we tend to think on asynchronous communications with pub/sub and req/reply over a
service bus implementation, with transports such as RabbitMq, AzureServiceBus or even Kafka and Kinesis streams.

In my mind, all these features are required upon scale, and we can have a simple lightweight abstraction layer
over a mediator pattern, running in memory that we can later move to a more complex async communication pattern with resilence patterns

## What should be included in a MVA

Think on the goal architecture, and start simplifying, adding minimum and lightweight abstraction layers, 
hidding implementation details and allowing future functionality additions with no interface changes.

My top list would be:

 * Authorization and authentication - It depends on the business, but if you can get rid of managing roles and permissions on MVP, please do.
 * Logging - Basic logging to troubleshoot issues
 * Decoupled domains with bullet proof interfaces - Avoid refactor nightmare
 * ServiceClient abstraction layer - No caching, simply start with CQRS over mediator bus - No http / service discovery.
 * Configuration - opinionated: Environment vars to the win!
 * Docker is a must for me, removes most of the burden of CI&CD

## What should not be included in MVA ?

Anything that adds un-needed complexity for MVP features and that does not add a future refactor risk and pain

My top list would be:

 * Microservices
 * Multi-repository
 * Async communication / buses
 * Complex roles and permissions systems
 * Internationalization and localization

## Reduce refactor risk

In order to minimize big refactor pains, using lightweight abstraction layers can avoid them
as long as their interfaces are designed abstracting yourself from the temporary solution. Many engineers define interface based on the implementation used, big mistake.

### Mediator and Service Client layers
For example if we aim to use bus for pub/sub or req/reply or send/forget, we can use mediator pattern as being an in-memory transport of our bus,
 this would allow to build the whole thing as a monolith instead of a distributed microservices, but still have decoupled services that communicate via service clients.

Service Clients on the other hand, are yet another abstraction layer on the communication pattern for domain-to-domain.
This service clients can use grpc, http or even bus messages, with or without caching. On MVA for MVP, we could get rid of all implementation details and just send commands&queries directly to the bus.

### Decouple domains
Decoupled domains reduce the risk of refactors, because their interfaces will be closed, and if we avoid mixing domains (very common among most engineers I know), we will be safe of big chunks of refactor in
domains which are not coupled to the business, like emailing, document storage, etc..

#### Example of mixed domains
I have seen presentations where the email service knew stuff about users and registration process, because there is a "welcome email".
This is mixing domains 101. The registration proccess belongs to the "User" or "Account" microservice, not the email service.

## Summary
 * Start your MVPs simple, with minimal devops needs
 * Monoliths are not the end of the world
 * Use mediator pattern for initial communication between domains
 * Spend time defining domains and interfaces to be truly decoupled
 * Remove time from designing the perfect architecture
 * Spend more time on the business features of the MVP
