---
title: Dynamic re-scaling in Strymon
permalink: /research/re_scaling.html
category: Components and use-cases
---

Streaming applications are by nature long-running, thus their workload and data input rate are largely unpredictable. Load and input rate variations significantly limit the ability of current systems to provide low-latency, robust, and low-cost solutions for streaming analytics.

We are studying the aspect of automatically provisioning resources for stateful streaming applications under dynamic workload and input conditions. Our preliminary research indicates that we can leverage traces obtained from application instrumentation to gain valuable insights about the performance behavior of a stream processing system. We plan to use such traces to build an evolving graph model of the system runtime performance and analyze it in real-time in order to support fully-automated elastic scaling of streaming computations. The performance model will be able to capture low-level runtime dependencies among parallel workers and their associated activities. The analysis of this graph-structured model will supply an in-depth understanding of the system state and enable fully automated scaling decisions for complex dataflow computations.

## Additional material

Moritz Hoffmann, Frank McSherry, Andrea Lattuada. [Latency-conscious dataflow reconfiguration](/assets/pdf/beyondmr18-hoffmann.pdf). **BeyondMR**, 2018

Vasiliki Kalavri, John Liagouris, Moritz Hoffmann, Desislava Dimitrova, Matthew Forshaw, Timothy Roscoe. [Three steps is all you need: fast, accurate, automatic scaling decisions for distributed streaming dataflows](/assets/pdf/osdi18-ds2.pdf) **OSDI**, 2018.
