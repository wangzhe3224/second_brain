---
title: "Learning Serverless - Design, Develop, and Deploy with Confidence"
date: 2023-02-08
tags: 
    - serverless
author: Jason Katzer
---

Cloud Native Computing Foundations (CNCF) defines `serverless` as:

> Serverless computing does not mean that we no longer use servers to host and
> run code; nor does it mean that operations engineers are no longer required.
> Rather, it refers to the idea that consumers of serverless computing no longer
> need to spend time and resources on server provisioning, maintenance, updates,
> scaling, and capacity planning. Instead, all of these tasks and capabilities
> are handled by a serverless plat‐ form and are completely abstracted away from
> the developers and IT/operations teams. As a result, developers focus on
> writing their applications’ business logic.

## Introduction

Strength of serverless:

- Increased scalability, security, and reliability
    - Infinite scalability. Netflix use is to process streaming
- Pay what you use
- Saving time and money on managing servers
- Improved developer productivity
    - ETL in serverless is amazing!
- Decreased management responsibilities
- Convenient integration

Weakness of serverless:

- Cold start
- Compute time
- VPC/Network issues
- Application size
- Potentially more expensive..
- Vendor lock-in
- Complex debuging

When to choose serverless?

- Tasks that can be broken up into small independent units of work
- Tasks that either have infrequent or unpredictable usage patterns
- Background work, or system to system communication that will not be impacted by cold starts

When not to choose serverless?

- The job is computation intensive and cannot be breakdown to smaller tasks

## Distributed Systems

A serverless system is inherently distributed. 
Given that a serverless function is by its very nature stateless, if your application involves any form of state, it is going to have to be distributed.

To design a good distributed system:

- Loose coupling ( decouple )
- Fault tolerance
- Generating unique keys
- Planing for idempotency

## Microservices

`microservices`: small, autonomous services that work together. By Sam Newman

When building your application in serverless, you will have to choose the
architectural patterns and practices that will allow your application to be
resilient and, more importantly, scale. 

How to build good microservices:

- Consistent Interfaces
- Loosely Coupled

## Serverless Architecture and Patterns

Cloud Provider Components:

- Streams
    - Use it when you want to react to events
    - Pattern: react to a change of database
    - Client usually manages a point to where are they
- Queues
    - A method to delay work
    - dead letter queue, to collect the failed messages
- Buckets
    - store arbitrary collection of files
- Compute
- Datastores
- Identity service
- API Gateway
- GraphQL
    - increasingly popular way to create a backend with the interface of a single database for applications that are in fact comprised of multiple services, each with its own unique datastores. 
- Networking
- State Machines
- Logging
- Monitoring and Alerting

Patterns:

- Serverless Monolith
    - API Gateway | Lambda | Datastore
- Incoming Webhook
- Generic Background Task Pattern
    - API Gateway | Lambda | Queue | Workers
- Streaming ETL
- Fanning Out
    - SNS | Multiple serverless functions

## Interfaces

- Automatic Retries and Dead Letter Queues
    - Dead letter queue
- Finite Versus Infinite Scale

Design Interfaces:

- Messages/Payloads
- Session and User/Auth
- Avoid Unbounded Requests
- Interface Versus Implementation
    - Avoid hidden coupling and interfaces
- Lines with Logic
    - Queue
    - Streams/Event Bus

Design the Unhappy Path:

- Validating Input
- Failures
   - Don’t fail silently 