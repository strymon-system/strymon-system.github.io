---
title: Interactive explanation of simulations
permalink: /research/output_explanation.html
category: Components and use-cases
---

Strymon is the first system to offer ***interactive explanations*** for the outputs of the user-defined dataflows it executes.

Consider a scenario where a simulation dataflow results in something that looks surprising. This may be due to some unnoticed properties of the workload used for the simulation, misconfigured simulation parameters, or even a logical bug in the dataflow itself. In such cases, Strymon allows its users to select individual records in the output of a simulation dataflow and ask for explanations of these records, i.e. for small subsets of the input data that contributed to having the 'strange' records in the output. 

The explanation mechanism of Strymon provides insights to the running dataflows, and is especially useful for debugging. It is applicable to arbitrary, even iterative dataflows, which may also include UDFs.


## Additional material

Zaheer Chothia, John Liagouris, Frank McSherry, Timothy Roscoe. [Explaining Outputs in Modern Data Analytics](http://www.vldb.org/pvldb/vol9/p1137-chothia.pdf). **PVLDB** 9(12):1137-1148, 2016.
