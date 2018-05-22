---
title: Advanced datacenter analytics in real time
permalink: /research/real_time_analytics.html
category: Components and use-cases
---

Our use case investigates the feasibility of ***online sessionization*** in a real enterprise datacenter. 

The basis for understanding the dynamics of a datacenter is
logging, and so many datacenters instrument their applications (and middleware) with functionality to emit log records
when messages are sent and/or received by each service or application process. 

The basic principle is to assign a unique ID to a request on its entry to the system and propagate (and
add to) this metadata whenever the request is passed between components. This leads to records of this form:

```js
Time: 2015/09/01 10:03:38.599859
Session ID: XKSHSKCBA53U088FXGE7LD8
Transaction ID: 26-3-11-5-1
`````

A common task then is to relate all pieces of work done
in individual components back to their originating request or
tenant (resource attribution). By combining the correlators
with other fields present in the logs, a detailed representation
can be re-built which contains all activities in the workflow
along with structure relating the individual facts. This task is known as sessionization, and is essentially implementeled in Strymon as a streaming `GROUP_BY`operation on the `Session ID`. 

The datacenter in our use case generates considerable logging traffic like the above – about 5Gb/s of network bandwidth on
average, or about 50TB of log data per day. Log generation itself is spread over about 1300 distinct log servers. 
For a real workload of 50 minutes, Strymon was able to perform online sessionization using only four commodity machines with 64G RAM.


## Additional material

Zaheer Chothia, John Liagouris, Desislava Dimitrova, Timothy Roscoe. [Online Reconstruction of Structural Information from Datacenter Logs](http://dl.acm.org/citation.cfm?id=3064195). **EuroSys**, 2017.
