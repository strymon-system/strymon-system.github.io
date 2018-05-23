---
title: Execution model 
permalink: /research/execution_model.html
category: Strymon
---

Strymon is a distributed system that builds on top of the [Rust](https://www.rust-lang.org/en-US/) prototype of [Timely Dataflow](https://github.com/frankmcsherry/timely-dataflow). Timely was first introduced in the [Naiad system](http://dl.acm.org/citation.cfm?id=2522738) and was an attractive option for us due to the following reasons:

* Timely adopts a pure *event-driven* and *push-based* execution model that is ideal for our setting where individual events coming from the datacenter trigger new Strymon computations in an asynchronous fashion. 

* Unlike other streaming platforms, Timely's model natively supports *arbitrary iterative computations*, which lie in the heart of simulating several complex datacenter operations, such as network routing. 

Strymon also relies on some features of [Differential Dataflow](https://github.com/frankmcsherry/differential-dataflow), which offers native support for *incremental execution* of dataflows. A promising application of this feature is described [here](incremental_routing.html).

## Strymon's publish-subscribe mechanism

Data sources in Strymon are modeled as typed data streams while data processing jobs are expressed in the form of timely dataflows. Each data stream can be forked and used by multiple dataflows simultaenously. Besides the input data streams, Strymon adopts an integrated publish-subscribe approach, which allows dataflow programs to also share their produced streams for consumption by other running dataflows, as it is shown below.

![Query 1]({{ "/assets/img/research/query1.svg" | absolute_url }})

Individual queries submitted to Strymon:
![Query 2]({{ "/assets/img/research/query2.svg" | absolute_url }})

Batch query execution:
![Query 2]({{ "/assets/img/research/q1q2.svg" | absolute_url }})

Reusing results of Strymon computations is especially helpful, not only for avoiding redundant work, but also for building complex simulations upon simpler ones. This is demostrated in various use cases, such as [this one](what_if). 


Additional information:

Sebastian Wicki. [An Online Stream Processor for Timely Dataflow](http://systems.ethz.pubzone.org/servlet/Attachment?attachmentId=3923&versionId=3508856). **Master Thesis**. Systems Group, Department of Computer Science, ETH Zürich, November 2016.

## Strymon's flow control mechanism

A major goal in Strymon's design and implementation is resource efficiency, with a particular focus on main memory requirements. 
Strymon prevents memory exhaustion by employing Faucet, a novel flow control mechanism that is applicable to arbitrary, even iterative, dataflows. Faucet allows Strymon users to drive the execution schedule of a computation by adding flow control points to selected parts of the running dataflows. 

Additional information:

Andrea Lattuada, Frank McSherry, Zaheer Chothia. [Faucet: A User-level, Modular Technique for Flow Control in Dataflow Engines](http://dl.acm.org/citation.cfm?id=2926544). **BeyondMR**, 2016.

Andrea Lattuada. [Programmable Scheduling in a Stream Processing System](http://systems.ethz.pubzone.org/servlet/Attachment?attachmentId=3746&versionId=3303163). **Master Thesis**. Systems Group, Department of Computer Science, ETH Zürich, September 2016.


