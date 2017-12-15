---
title: Run-Time Architecture
nav-index: 4
category: Strymon Core
---

Strymon is a distributed system that builds on top of the [Rust](https://www.rust-lang.org/en-US/) prototype of [Timely Dataflow](https://github.com/frankmcsherry/timely-dataflow). Timely was first introduced in the [Naiad system](https://dl.acm.org/citation.cfm?id=2522738) and was an attractive option for us due to the following reasons:

  1. Timely adopts a pure event-driven and push-based execution model that is ideal for our setting where individual events coming from the datacenter trigger new Strymon computations in an asynchronous fashion.
  2. Unlike other streaming platforms, Timely’s model natively supports arbitrary iterative computations, which lie in the heart of simulating several complex datacenter operations, such as network routing.

Strymon also relies on some features of [Differential Dataflow](https://github.com/frankmcsherry/differential-dataflow), which offers native support for incremental execution of dataflows.

## Execution Platform

The `strymon-core` repository contains the Strymon run-time. It manages the submission and execution of
Timely Dataflow processing *jobs* on a dynamic cluster of machines. For a machine to participate
in the cluster, it needs to run an *executor* process. The whole system is managed by a central
process called the *coordinator*, which maintains the current state of the system in a data structure
called the *catalog*. New jobs are always submitted to the coordinator, which forwards the requests
to the chosen executors.

### Glossary

The following glossary provides a brief overview of the different components of the Strymon run-time:

Job 
: A user-submitted Timely dataflow program running in the system, which might spawn over multiple machines and threads. Progress tracking is available within the domain of a job through the mechanisms provide by Timely dataflow. It enables communication and synchronization between the workers of a single job. Communication between jobs is achieved through the publish-subscribe mechanism discussed below. Historically also "jobs" have been called "queries", a naming scheme which is still apparent in the source code.

Worker
: A thread belonging to a query, driving the computation of its local dataflow graph instance. The number of workers defines the level of parallelism of a job.

Executor
: The process running on each machine responsible for running job processes. Every executor which joins the system registers itself at the coordinator.

Catalog
: Data structure storing and exposing the system state at the coordinator. It stores information about all currently running jobs, the available executors, as well as all publication and subscriptions on topics.

Topic
: A named and typed representation of a published data stream. Each topic consists of a name, a unique identifier and the address of its publisher.

Publisher
: Job producing data which exposes its internal Timely streams as a topic to downstream subscribers.

Subscriber
: A Timely dataflow job consuming the data from a topic it has subscribed to. By subscribing to a topic, a job will observe all events published since it joined. Jobs can subscribe to multiple topics at the same time.

![Overview of the components of the Strymon run-time]({{ "/assets/docs/strymon-runtime-components.svg" | absolute_url }})

*Figure*: *Jobs* (dashed boxes) consist of one or more *worker* threads (rounded grey boxes) driving the dataflow computation. A job might
span over multiple *executors*, makeing use of the network for message exchanges between the workers of a job.
The *coordinator* maintains a connection to every executor and every job process. The state of the whole system is stored in the *catalog*.

## Publish-Subscribe Mechanism

While data processing jobs are expressed in the form of timely dataflows, data sources in Strymon are modeled as typed data streams. Strymon adopts an integrated publish-subscribe approach, which allows dataflow programs to also share their produced streams for consumption by other running dataflows.

Topics currently come in two different flavors:

 - *Stream topics*: Stateless, asynchronous topics publishing the contents and progress tracking information of a Timely stream. Data produced before a consumer subscribed to a certain topic will not be received by that receiver.
 - *Collection topics*: Stateful topics which expose state in form of a multiset. The contents of the multiset and all subsequent updates is observed as a stream of insertions and removals by its subscribers. The collectoin publisher ensures that late subscribers get a consistent view of the current state upon receiving the subscription request.

#### Additional information:

Sebastian Wicki. [An Online Stream Processor for Timely Dataflow](http://systems.ethz.pubzone.org/servlet/Attachment?attachmentId=3923&versionId=3508856). **Master Thesis.** Systems Group, Department of Computer Science, ETH Zürich, November 2016.

