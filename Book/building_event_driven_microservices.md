---
title: Building Event-Driven Microservices
tags:
    - event-driven
    - microservices
---

Definition of Event-Driven Microservice:

( ahha! Event becomes the interfaces )

These services consume events from input event streams; apply their specific business logic; and may emit their own output events, provide data for request-response access, communicate with a third-party API, or perform other required actions.


- [Why?](#why)
  - [Communication Structure](#communication-structure)
  - [Asynchronous Event-Driven Microservices](#asynchronous-event-driven-microservices)
- [EDM Fundamentals](#edm-fundamentals)
  - [The Structure of an Event](#the-structure-of-an-event)
  - [Event Definitions and Schemas](#event-definitions-and-schemas)
  - [Single Writer Principle](#single-writer-principle)
  - [Event Broker](#event-broker)
  - [Managing Microservices at Scale](#managing-microservices-at-scale)
- [Communication and Data Contracts](#communication-and-data-contracts)
- [Integrating Event-Driven Architectures with Existing Systems](#integrating-event-driven-architectures-with-existing-systems)
- [Event-Driven Processing Basics](#event-driven-processing-basics)
- [Deterministic Stream Processing](#deterministic-stream-processing)
  - [Determinism with Event-Driven Workflows](#determinism-with-event-driven-workflows)
  - [Timestamps](#timestamps)
  - [Event Scheduling and Deterministic Processing](#event-scheduling-and-deterministic-processing)
  - [Watermarks](#watermarks)
  - [Stream Time](#stream-time)
  - [Out-of-Order and Late-Arriving Events](#out-of-order-and-late-arriving-events)
  - [Handle Late Events](#handle-late-events)
- [Stateful Streaming](#stateful-streaming)
  - [State Stores and Materializing State](#state-stores-and-materializing-state)
  - [Recording State to a Changelog Event Stream](#recording-state-to-a-changelog-event-stream)
  - [Materializing State to an Internal State Store](#materializing-state-to-an-internal-state-store)
  - [Materializing State to an External State Store](#materializing-state-to-an-external-state-store)
- [Building Workflows with Microservices](#building-workflows-with-microservices)
  - [The Choreography Pattern](#the-choreography-pattern)
  - [The Orchestration Pattern](#the-orchestration-pattern)
  - [Distributed Transactions](#distributed-transactions)
- [Microservices Using Function-as-a-Service](#microservices-using-function-as-a-service)
  - [Designing Function-Based Solutions as Microservices](#designing-function-based-solutions-as-microservices)
    - [Ensure Strict Membership to a Bounded Context](#ensure-strict-membership-to-a-bounded-context)
    - [Commit Offsets Only After Processing Has Completed](#commit-offsets-only-after-processing-has-completed)
    - [Less Is More](#less-is-more)
  - [Choosing a FaaS Provider](#choosing-a-faas-provider)
  - [Building Microservices Out of Functions](#building-microservices-out-of-functions)
  - [Cold Start and Warm Starts](#cold-start-and-warm-starts)
  - [Starting Functions with Triggers](#starting-functions-with-triggers)
  - [Performing Business Work with Functions](#performing-business-work-with-functions)
  - [Maintaining State](#maintaining-state)
  - [Functions Calling Other Functions](#functions-calling-other-functions)
  - [Termination and Shutdown](#termination-and-shutdown)
  - [Tuning Your Functions](#tuning-your-functions)
- [Basic Producer and Consumer Microservices](#basic-producer-and-consumer-microservices)
- [Heavyweight Framework Microservices](#heavyweight-framework-microservices)
- [Lightweight Framework Microservices](#lightweight-framework-microservices)
  - [Choosing a Lightweight Framework](#choosing-a-lightweight-framework)
- [Integrating Event-Driven and Request-Response Microservices](#integrating-event-driven-and-request-response-microservices)
- [Tooling](#tooling)
- [Testing](#testing)
- [Deployment](#deployment)
  - [Principles of Microservice Deployment](#principles-of-microservice-deployment)
  - [Architectural Components of Microservice Deployment](#architectural-components-of-microservice-deployment)
  - [The Basic Full-Stop Deployment Pattern](#the-basic-full-stop-deployment-pattern)
  - [The Rolling Update Pattern](#the-rolling-update-pattern)
  - [The Breaking Schema Change Pattern](#the-breaking-schema-change-pattern)
  - [The Blue-Green Deployment Pattern](#the-blue-green-deployment-pattern)
  - [What systematic trading need for deployment??](#what-systematic-trading-need-for-deployment)
- [Conclusion](#conclusion)
  - [Communication Layers](#communication-layers)
  - [Business Domains and Bounded Contexts](#business-domains-and-bounded-contexts)
  - [Shareable Tools and Infrastructure](#shareable-tools-and-infrastructure)
  - [Schematized Events](#schematized-events)
  - [Data Liberation and the Single Source of Truth](#data-liberation-and-the-single-source-of-truth)
  - [Microservices](#microservices)
  - [Microservice Implementation Options](#microservice-implementation-options)
  - [Testing](#testing-1)
  - [Deploying](#deploying)


## Why?

__Domain Driven Design__ introduces some of the necessary concepts for building event-driven microservices:

- Problem Space
    - Domain := everything business must contend with
        - Rules
        - Processes
        - Ideas
        - Terminology
        - other stuff related to business
    - Subdomain := a component of the main domain
- Solution Space
    - (Sub)domain model := an abstraction of the actual domain useful for the business purposes
    - Bounded context := the logical boundaries, including inputs, outputs, requirement, data models. The majority of communication should be within context, __not cross boundaries__.

Guide lines:

- _Bounded contexts should be highly cohesive._
- _Connections between bounded contexts should be loosely coupled_
- _Bounded contexts are established around these subdomains and form the basis for the creation of microservices._

Aligning bounded contexts on business requirements allows teams to make changes to microservice implementations in a loosely coupled and highly cohesive way. Aligning microservices on technical requirements is problematic.

Modeling event-driven microservices architectures around business requirements is preferred, though there are tradeoffs with this approach.

Code may be replicated a number of times, and many services may use similar data access patterns.

### Communication Structure

(What about shared data for different microservices?)

An event-streaming data communication structure decouples the production and ownership of data from the access to it. Services no longer couple directly through a request-response API, but instead through event data defined within event streams.

Events are:

- the data, they act both as data storage and as a mean of asynchronous communication between services.
- single source of truth
- consumers perform their own modelling and query

### Asynchronous Event-Driven Microservices

Primary benefits:

- Granularity. Can be easily rewritten when busi‐ ness requirements change.
- Scalability. Individual services can be scaled up and down as needed.
- Technological flexibility. This also allows for easy prototyping using pioneering technology.
- Business requirement flexibility
- Loosely coupling. Event-driven microservices are coupled on domain data NOT on a specific implementation API. ( __Also means data modeling and management is important__)
- Continuous delivery support
- High testability

( Question: What defines a microservice? ) 

## EDM Fundamentals

Running microservices at any meaningful scale often necessitates using deployment pipelines and container management systems.

__Microservice Topology__ is the event-driven topology internal to a single microservice. It defines the data-driven operation to be performed on incoming events, including transformation, storage, and emission.

![](2023-01-10-23-15-56.png)

__Business Topology__ is the set of microservices, event streams, and APIs that fulfill complex business functions. 

![](2023-01-10-23-17-51.png)

### The Structure of an Event

Events must contain all the information required to accurately describe what happened.

Typically, events are represented using a key/value format.

- Unkeyed event, singular statement of fact.
- Entity event, keyed by a unique id and the represents an unique thing.
- Keyed event

__Entity events are particularly important in event-driven architectures. They provide a continual history of the state of an entity and can be used to materialize state.__  You can materialize a stateful table by applying entity events, in order, from an entity event stream. 

The deletion of a keyed event is handled by producing a tombstone.

All of these systems require that you have the ability to materialize streams of events into current state representations. 

> This book uses online shopping as examples for a lot of concepts.

### Event Definitions and Schemas

it is important that both the producers and consumers of events have a common understanding of the meaning of the data. 

- Avro
- Protobuf

### Single Writer Principle

Each event stream has one and only one producing microservice.

### Event Broker

At the heart of every production-ready event-driven microservice platform is the event broker.

Benefits:

- Scalability
- Durability
- High availability
- High-performance

These are the minimal requirements of the underlying storage of the data by the broker:

- Partitioning
- Strict ordering
- Immutability
- indexing
- infinite retention
    - long term storage
    - short term storage
- replayability

### Managing Microservices at Scale

Containerization and container management systems allow for running microservices at scale.

## Communication and Data Contracts

Design principles of Events:

- Use explicit schema definition as contract
- Tell the truth, nothing else
- Use singular event definition per stream
- Use the narrowest data types
- Keep event single-purpose
- Minimize the size of events
- Involve prospective consumers
- Avoid events as Semaphores
    - including the result as well

Event Format:

- Json
- Protobuf
- Avro

## Integrating Event-Driven Architectures with Existing Systems

todo

## Event-Driven Processing Basics

- Composing stateless topologies
    - transforms
        - filter
        - map
        - map value
        - custom transform
    - branch and merge
- Re-partition of the streams
- Co-partition of the streams
- Partition assignment algorithm
    - ensure that partitions are evenly distributed across the consumer instances, assuming that the consumer instances are equal in processing capabilities
- Recovering from Stateless Processing Instance Failures
    - Replay the events streams

## Deterministic Stream Processing

3 questions:

- How does a microservice choose the order of events to process when consuming from multiple partitions
- How does a microservice handle out-of-order and late-arriving events
- How do we ensure that our microservices produce deterministic results when processing streams in near–real time versus when processing from the beginning of the streams

### Determinism with Event-Driven Workflows

Be sure to pay attention to any nondeterministic operations in your workflow, such as wall-clock, external API calls.

What hurts deterministic processing?

- latency
- producer/consumer failures
- network issues

the reality is that our services can only achieve a best effort at determinism. how to:

- consistent timestamps
- well-selected event keys
- partition assignment
- event scheduling
- strategies to handle late-arriving events

### Timestamps

> A fundamental limitation of physics is that two independent systems cannot be guar‐ anteed to have precisely the same system-clock time. 

Event stored in an event stream has both an offset and a timestamp.

- event time: local ts assigned by producer at the time event occurs
- broker ingestion time
- consumer ingestion time
- processing time

![](2023-01-11-16-19-44.png)

### Event Scheduling and Deterministic Processing

Event scheduling is the process of selecting the next events to process when consuming from multiple input partitions.

### Watermarks

Watermarking is used to track the progress of event time through a processing topology and to declare that all data of a given event time (or earlier) has been processed.

A watermark is a declaration to downstream nodes within the same processing topology that all events of time t and prior have been processed. 

### Stream Time

### Out-of-Order and Late-Arriving Events

An event is late only when it has missed a deadline specific to the consumer.

Late events are predominantly the concern of time-based business logic, such as aggregating events in a particular time period or triggering an event after a certain period of time has passed. 

Windowing means grouping events together by time:

- Tumbling Window, window with fixed size (time interval)
- Sliding Window, 
- Session Window,

### Handle Late Events

- Drop
- Wait
- Grace Period

## Stateful Streaming

### State Stores and Materializing State

__Materialized state__ is a projection of events from the source event stream ( Immutable )

__State store__ is where business state is stored ( Mutable )

Two ways to store the state:

- internal
- external

![](2023-01-11-16-42-10.png)

### Recording State to a Changelog Event Stream

A __changelog__ is a record of all changes made to the data of the state store.

### Materializing State to an Internal State Store

### Materializing State to an External State Store

## Building Workflows with Microservices

A `workflow` is a particular set of actions that compose a business process, including any logical branching and compensatory actions. 

Four aspect to think about EDM workflows (patterns):

- Create
- Modify
- Monitor
- Implement distributed transactions

### The Choreography Pattern

Choreography (reactive architectures) is common in event-driven microservice architectures,which refers to highly decoupled microservice architectures.
The upstream business logic is fully isolated from the downstream consumers.

Choreography is desirable in the majority of interteam communications, as it allows for loosely coupled systems and reduces coordination requirements. 

This choreographed workflow is a form of emergent behavior, where it is not just the individual microservices that dictate the workflow, but the relationships between them as well.

### The Orchestration Pattern

In the orchestration pattern a central microservice, the orchestrator, issues commands to and awaits responses from subordinate worker microservices.

![](2023-01-11-22-58-57.png)

![](2023-01-11-22-59-43.png)

### Distributed Transactions

A `distributed` transaction is a transaction that spans two or more microservices. 

It is best to avoid implementing distributed transactions. 

## Microservices Using Function-as-a-Service

Functions-as-a-Service (FaaS) is a “serverless” solution that has become increasingly popular in recent years. 

### Designing Function-Based Solutions as Microservices

- Design bounded context
- Design triggers
- Design State management
- Implement business functionality
- Function Details
    - Select FaaS provider
    - Cold and Warm start
    - Resource tuning

#### Ensure Strict Membership to a Bounded Context

It’s important to identify which function belongs to which bounded context, because the high granularity of functions can blur those lines.

Some practical ways of maintaining bounded contexts with functions include:

- Ensure that data stores are kept private from external contexts.
- Use standard request-response or event-driven interfaces when coupling to other contexts.
- Maintain strict metadata around which function belongs to which context (a 1:1 mapping of function to product).
- Maintain function code within a repository mapped to the bounded context.

#### Commit Offsets Only After Processing Has Completed

Offsets are committed at one of two times: when the function starts or when the function has completed its processing. 

#### Less Is More

A good rule of thumb for FaaS solutions is that fewer functions are better than many granular functions.

### Choosing a FaaS Provider

Just like event brokers and container management system (CMS), FaaS is another thing to choose. 

### Building Microservices Out of Functions

- Function
- Input event stream
- Trigger logic
- Error, Scaling policies, with metadata
    - Consumer group
    - Consumer properties, such as batch size and batch window
    - Retry and error handling policies
    - Scaling policies

![](2023-01-11-23-20-16.png)

### Cold Start and Warm Starts

A cold start is the default state of the function upon starting for the first time, or after a sufficient period of inactivity. 

### Starting Functions with Triggers

Triggers are used to tell a function to start up and begin processing.

- based new events
    - Synchronous
    - Async
- based on consumer group lag
- based on schedule
- based webhooks
- based on resource event
  
### Performing Business Work with Functions

What kind of work is good for FssS:

- Simple topology
- Stateless
- No deterministic processing multiple events
- No concurrency
- Need horizontal scaling
- On-demand

### Maintaining State

- Functions are short lived
- Need external stateful service, ie a database service
    - Azure durable function is function with state managed

### Functions Calling Other Functions

Function communication methods:

- asynchronously with events
- sync with API calls

![](2023-01-12-13-43-18.png)

![](2023-01-12-13-43-45.png)

![](2023-01-12-13-44-06.png)

### Termination and Shutdown

Function execution is between 0 to 10 minutes. 

### Tuning Your Functions

- Allocate sufficient resources
    - CPU
    - Memory
    - IO
- Batch event processing parameters
    - max execution time
    - max batch size

## Basic Producer and Consumer Microservices

Basic producer and consumer (BPC) microservices ingest events from one or more event streams, apply any necessary transformations or business logic, and emit any necessary events to output event streams. 

BPC works well if:

- Integration with Existing and Legacy Systems
    - Sidecar pattern
- Stateful Business Logic That Isn’t Reliant Upon Event Order
- When the Data Layer Does Much of the Work
- Independent Scaling of the Processing and Data Layer


## Heavyweight Framework Microservices

`heavyweights streaming frameworks`:

- Spark
- Flink
- Storm
- Heron
- Beam

Why it is heavy?

- it requires an independent cluster of processing resources to perform its operations. 
- it use own internal mechanisms for handling failures, recovery, resource allocation, task distribution, data storage, communication, and coordination between processing instances and tasks.

This is in contrast to the lightweight framework, __FaaS, and BPC implementations that rely heavily on the container management system (CMS) and the event broker for these functions__.

## Lightweight Framework Microservices

It leverage heavily with __event broker__ and __container management system (CMS)__.

- state management with `changelog`

### Choosing a Lightweight Framework

- Apache kafka stream
- Apache Samza

## Integrating Event-Driven and Request-Response Microservices

## Tooling

## Testing

- Functional testing
    - unit
    - integration
    - system
    - regression
    - topologies test ( this is kind of special to EDM? )
- Non-functional testing
    - performance
    - load 
    - stress
    - recovery

## Deployment

### Principles of Microservice Deployment

- Give teams deployment autonomy
- Implement a standardized deployment process
- Provide necessary supportive tooling
- Consider event stream reprocessing impacts
- Adhere to service-level agreements (SLAs)
- Minimize dependent service changes
- Negotiate breaking changes with downstream consumers

### Architectural Components of Microservice Deployment

### The Basic Full-Stop Deployment Pattern

### The Rolling Update Pattern

### The Breaking Schema Change Pattern

### The Blue-Green Deployment Pattern

### What systematic trading need for deployment??

## Conclusion

### Communication Layers
### Business Domains and Bounded Contexts
### Shareable Tools and Infrastructure
### Schematized Events
### Data Liberation and the Single Source of Truth
### Microservices
### Microservice Implementation Options
### Testing
### Deploying