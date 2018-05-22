---
title: Research overview
permalink: /research/
nav-index: 0
---

Strymon (pronounced "stream-on") is a novel system for carrying out online analyses and simulations of large datacenters. 

Strymon leverages existing logging and monitoring pipelines of modern production datacenters to ingest cross-layer events in a streaming fashion and predict possible effects of such events in what-if scenarios by simulating the *hypothetical datacenter state* alongside the real one. 

Strymon's ultimate goal is to drive research and innovation in datacenter management. It advances the current practices in that the hypothetical datacenter state is fully exposed to Strymon users in a timely manner (as well-defined data streams), enabling the composition of deep analytics and cross-layer simulations in real time and within the same execution engine.

## Support

<div>
    <div style="display: inline-block; margin-right:50px;">
        <img style="width: 200px;" src="/images/amadeus_logo.svg" alt="Amadeus"/>
    </div>
    <div style="display: inline-block; margin-right:50px;">
         <img style="width: 250px;" src="/images/snf_logo.png" alt="SNF"/>
    </div>
    <div style="display: inline-block; margin-right:50px;">
        <img style="width: 200px;" src="/images/google_logo.png" alt="Google"/>
    </div>
</div>



## Strymon's architecture

{% include image.html file="strymon.svg" max-width="840" alt="Strymon Architecture" %}

Strymon's design and implementation is driven by the following needs:

 * **Low latency - High throughput.** Strymon focuses on real-time data processing over thousands of input streams, e.g. logs of events coming from the datacenter. Modern datacenters are heavily instrumented and produce TBs of event logs within minutes; in this setting, fast reaction to changes in the behavior of the datacenter requires that vast amounts of events are being processed *online*, while the logs are generated, kept for a while and then dropped. To meet this need, Strymon adopts a pure [event-driven streaming execution model](execution_model.html), which provides interactive response times, even for complex *iterative* data processing pipelines.
 
 * **Scale up and out.** Strymon can be deployed in clusters of a modern multi-core machines with zero configuration cost. Any dataflow program in Strymon is *transparently* parallelized using the available *workers* (threads/processes) as specified by the user. Currently, the number of available workers is statically defined (upon start-up) but [dynamic re-scaling in Strymon is the focus of our ongoing research](dynamic_rescaling.html).

 * **Reliability.** Strymon focuses on continuous mission-critical management tasks in the datacenter; therefore, it is designed and implemented as a system that is *always up and running*. For small datacenters, Strymon can be used as a rack-scale system but we also found cases where it needs to be deployed in large clusters, e.g. for scaling out long-running network analytics. In this setting, [fast recovery from system failures is of utmost importance for Strymon's adoption in practice, and is something we are currently investigating](fault_tolerance.html).

 * **Expressivity.** Strymon's programming framework supports a wide range of datacenter management tasks, from standard performance monitoring to complex network simulations with dynamic routing algorithms and advanced load balancing techniques. Strymon users can express jobs as [timely dataflows](execution_model.html) with an imperative language. Recently, we have started experimenting with high-level declarative languages to facilitate datacenter analytics and certain management tasks like resource virtualization.

 * **Modularity.**  Strymon adopts a neat [publish-subscribe system paradigm](execution_model.html#strymons-publish-subscribe-mechanism) where data sources (streams) and jobs (timely dataflows) can be registered and de-registered dynamically at runtime. Composability of dataflows is natively supported by Strymon as a means to facilitate programming and reuse of intermediate analysis results.
 
 [comment]: and routing policy definition.

 * **Modest use of resources.** Strymon can comfortably keep up with fast and possibly unbounded input streams of data with modest computational and memory resources. In real-world scenarios provided by our industry partners, there is the need to [continuously process more than 1000 distributed log streams with high input rates, using tiny clusters of up to 10 commodity machines](real_time_analytics.html).
    
 * **Ease of use.** Strymon aims to serve as a useful tool for datacenter administrators. Besides easy deployment and tuning, its success also lies in intuitive programming APIs, meaningful visualizations, and good support for [debugging](output_explanation.html). 

[comment]: To the best of our knowledge, Strymon is the first dataflow system that can transparently [profile the computations it performs](critical_path.html) as well as [explain their individual outputs](output_explanation.html), both in real time and with negligible overhead in performance.
[comment]: In case you are wondering, Strymon is named after [this river](https://en.wikipedia.org/wiki/Struma_(river)).
[comment]: ## Getting started
[comment]: To get started with Strymon, see [Getting Started][index].

