---
title: Data model
permalink: /research/data_model.html
category: Strymon
---

Strymon brings innovation to datacenter management by introducing a formal data model as a common basis for understanding and simulating diverge operations in the datacenter. 

This formal model is based on the concept of *property graphs*, which are used to fuse different types of data collected from the datacenter (relational, semi-structured, text, etc.) into a uniform representation. Property graphs in Strymon capture structural and behavioural properties of the various datacenter components, and provide the necessary abstractions to help operators evaluate *hypotheses* (alternative behaviour) and *conditions* (expected behaviour) in a prinicipled manner.

## Property graphs

A property graph *G* in Strymon has these elements:

* A set of vertices *V*, each one having a unique ID, a set of outgoing edges, a set of incoming edges, and a collection of properties (attributes).

* A set of edges *E*, each one having a unique ID, a label, a couple of end vertices, and a collection of properties (attributes).

An example property graph from Strymon's [incremental routing use case](incremental_routing.html) models the network topology as a graph *G* = (*V*, *E*), where:

* *V* is the set of vertices (hosts and switches), each with a unique ID and zero or more properties relevant to routing, such as the online status of a switch.

* *E* is the set of edges representing physical links identified by their endpoints. Each edge has an associated weight *w* used to denote bandwidth, which is used by the incremental routing algorithm.

![An example Property Graph]({{ "/assets/img/research/pg-example.jpeg" | absolute_url }})

A core feature of Strymon's property graphs is that elements are also associated with a delta value δ ∈ {−1,+1} that is used to represent changes in the graph *G*. 

In the previous example, link and switch additions are equivalent to adding edges with δ = +1, while removals are expressed as sets of edges with negative delta values (δ = -1). Removing a switch, let *s*, is simply done by removing all edges with *s* as an endpoint. Also, weight changes are modeled as edge removals followed by an edge addition with the updated weight. 

As our use cases demonstrate, this modeling approach greatly facilitates the definition of hypothetical scenarios in simulations, and also simplifies Strymon's [execution model](execution_model.html).
