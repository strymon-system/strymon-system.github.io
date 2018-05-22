---
title: Online critical path analysis of dataflow systems
permalink: /research/critical_path.html
category: Components and use-cases
---

Strymon is the first system that supports ***online critical path analysis*** of modern dataflow systems like [Spark](https://spark.apache.org/), [Flink](https://flink.apache.org/) and  [TensorFlow](https://www.tensorflow.org/). Such distributed systems are heavily used for data processing in large enterprise datacenters and their performance is crucial to the overall performance of the datacenter.

Understanding the performance of distributed dataflows is hard. In systems like the above, the  computation  of  several  parallel  processes  is
interleaved  with  data  and  control  communication,  and  execution dependencies typically span multiple system components.  In  such  an  environment,  bottleneck  detection  is cumbersome and currently relies heavily on humans. 
After decades of systems research, the state-of-the-art in performance analysis still relies on offline trace processing, thus
it is only suitable for batch computations and post-mortem reports.  

Strymon's novel approach towards online performance analysis is implemented in a separate system module called SnailTrail. SnailTrail can identify bottlenecks in real time and make automated optimization possible at runtime. One prominent use case of SnailTrail is the online profiling of Strymon itself, which helps users identify bottlenecks in their simulations. 


## Additional material


Moritz Hoffmann, Andrea Lattuada, John Liagouris, Vasiliki Kalavri, Desislava Dimitrova, Sebastian Wicki, Zaheer Chothia, Timothy Roscoe. [SnailTrail: Generalizing Critical Paths for Online Analysis of Distributed Dataflows](pdf/nsdi18-hoffmann.pdf). **NSDI '18**. Source code is available on [Github](https://github.com/strymon-system/snailtrail).

Ralf Sager, John Liagouris, Desislava Dimitrova, Andrea Lattuada, Vasiliki Kalavri, Zaheer Chothia, Moritz Hoffmann, Timothy Roscoe. [SnailTrail: Online Bottleneck Detection for your Dataflow](https://eurosys2017.github.io/assets/data/posters/poster13-Sager.pdf). **EuroSys**, 2017. 

Ralf Sager. [Real-Time Performance Analysis of a Modern Data-parallel Stream Processing Engine](pdf/timely_cpath_thesis.pdf). **Master Thesis**. Systems Group, Department of Computer Science, ETH Zürich, September 2016. 

