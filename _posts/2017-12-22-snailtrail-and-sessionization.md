---
layout: post
title: 🎁 Strymon Holiday Season Release 🎁
subtitle: Open-Source Release of SnailTrail and Sessionization
---

We are very happy to announce our last release within the Strymon project for this year: The open-source release of [SnailTrail](https://github.com/strymon-system/snailtrail) and our work on [online sessionization and structural reconstruction](https://github.com/strymon-system/reconstruction).

## Understanding Distributed Applications with Strymon

Distributed applications and frameworks are often heavily instrumented to provide detailed traces of their execution. These sources have proven crucial for understanding systems behavior and debugging performance issues. With Strymon, our goal is to provide these insights to users in real-time, allowing them to take appropriate actions as quickly as possible.

Today, we are announcing the open-source release of two projects which have been developed for exactly this purpose as part of Strymon: The work on [online reconstruction](http://strymon.systems.ethz.ch/real_time_analytics.html) of structural information from datacenter logs and the source code of [SnailTrail](http://strymon.systems.ethz.ch/critical_path.html), our most recent work on understanding the performance of distributed dataflows.

### Online Reconstruction of Structural Information from Datacenter Logs

  - **GitHub Repository: [strymon-system/reconstruction](https://github.com/strymon-system/reconstruction)**
  - **Documentation:** [Website](https://strymon-system.github.io/docs/reconstruction/introduction)
  - **Paper**: [EuroSys '17](https://people.inf.ethz.ch/zchothia/papers/online-reconstruction-eurosys17.pdf)

The open-source release of this work contains the building blocks to construct your own custom pipeline reconstructing information about the user sessions in your datacenter applications. We provide the Timely Dataflow operators required to perform online sessionization on log traces, as well an example program which shows you how to use the resulting output to perform online analytics on user sessions, such as the extraction of trace-tree shapes. Please also consult our [documentation](https://strymon-system.github.io/docs/reconstruction/concepts) for more details on how to apply these tools for your own purposes.

### SnailTrail: Generalizing Critical Paths for Online Analysis of Distributed Dataflows

With our paper being accepted at *NSDI '18*, we are also very happy to release the source code of SnailTrail, our system for critical path analysis for understanding the performance of distributed dataflow computations:

  - **GitHub Repository: [strymon-system/snailtrail](https://github.com/strymon-system/snailtrail)**
  - **Paper**: [NSDI '18 (Draft)](http://strymon.systems.ethz.ch/pdf/snailtrail_draft.pdf)

The released source code contains the libraries for construing the program activity graph of a running dataflow computations, as well as our online algorithm for computing the *critical participation* and other performance metrics. We also include the tools and libraries for writing, reading, and visualizing message traces in our common log format. The repository contains prototypes for converting traces from Spark's and TensorFlow's existing instrumentation into the SnailTrail's common log format. Our custom instrumentation for other distributed dataflow systems will follow in a future release.

---

Stay tuned for more updates from us in 2018 -- and as always, please do not hesitate to contact us at <a href="mailto:strymon-users@lists.inf.ethz.ch">strymon-users@lists.inf.ethz.ch</a> for any questions or comments.
