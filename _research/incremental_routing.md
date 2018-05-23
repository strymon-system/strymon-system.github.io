---
title: Fast incremental routing for SDNs
permalink: /research/incremental_routing.html
category: Components and use-cases
---

Network routing is inherently dynamic.  Many factors that affect routing change frequently: the underlying topology (due to failures, reconfigurations, and provisioning), the set of routing policies that constrain where traffic flows, and the set of active flows themselves.
Traditional decentralized routing protocols such as OSPF are designed to cope with this dynamism by route recomputation, either periodically
or in response to changes.

Software-defined networking (SDN) replaces this with logically centralized route computations, offering greater flexibility and control over network policies, and more efficient network resource usage. A downside, however, is that the routing itself may become a bottleneck. For one, high *latency* in making routing decisions could lead to traffic loss as queues at switches start to build. Then, low *throughput* of routing translates to sub-optimal network utilization. 

We developed a routing module by leveraging a novel design approach. In particular, we express routing as an incremental dataflow computation on a stream of network updates. Furthermore, Strymon's routing prototype proactively runs all-pairs shortest path, instead of reactively computing single-source shortest path per flow request as most SDN controllers do. As results, Strymon's module has latency an order of magnitute lower compared to ONOS, a state-of-the-art controller. For a FatTree topology with 3K switches, ONOS takes 36ms to re-route a single flow after a network change, e.g., failed link. Strymon takesn only 2.6ms to re-route all flows affected by the change.

## Additional Material

Christian Stücklberger. [Expressing the Routing Logic of a SDN Controller as a Differential Dataflow](/assets/pdf/sdn_thesis.pdf). **Master Thesis**. Systems Group, Department of Computer Science, ETH Zürich, September 2016.
