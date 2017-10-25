---
layout: docs
title: Running the example
nav-index: 2
---

The Strymon source code ships with a simple dataflow which generates a
small datacenter network topology and allows for the simulation of switch
and link failures.

The source code of the example can be found in the `apps` folder. It consists
of the following components:

#### Topology Generator

This program generates a random datacenter network topology and publishes it
in a topic called `topology`. This program is able to inject random network
faults into the simulated topology.

#### Connected Components

This dataflow is subscribed to changes to the network topology and runs an
incremental implementation of connected components to automatically detect
partitions in the simulated network. The implementation is based on
[Differential Dataflow](https://github.com/frankmcsherry/differential-dataflow).

## Step 1: Generating a network topology

First, please ensure that the Strymon cluster is running locally as described
in the [Getting started](/docs/getting-started) tutorial. To start the topology
generator, submit it as follows using the `strymon` command-line tool:

```terminal
$ ./bin/strymon submit apps/topology-generator
Building job binary with `cargo build` (this will take a while)
Submitting binary "target/release/main"
Successfully spawned job: 0
```

By default, it will generate a 16-ary [Fat-tree network topology](https://en.wikipedia.org/wiki/Fat_tree)
and report the number of generated switches and links. You can follow the output
of all running programs by inspecting the executor log file:

```terminal
$ tail -f logs/executor_localhost.log           
INFO:…: QueryId(0) | Hosts: 1024, Switches: 320, Ports: 16, Links: 2048
```

## Step 2: Running connected components

Connected components allows us to keep track of the number of network partitions
in the generated topology. It listens for changes to links or switches in the
simulated network and automatically recomputes the connectivity of all nodes.

```terminal
$ ./bin/strymon submit apps/connected-components
…
Successfully spawned job: 1
```

In the logs you will see the output of the new job. The initially generated
network forms a connected graph:

```
INFO:…: QueryId(1) | All nodes in the graph are now connected.
```

## Step 3: Injecting network faults

The topology generator allows you to inject temporary faults into the network.
Using the provided script, you can disconnect a random switch for 10 seconds
by running the following command:

```terminal
$ ./apps/topology-generator/inject-fault.sh disconnect-random-switch
OK
```

In the logs, you should first see the topology generator disconnecting this
random switch, and immediately afterwards you will see the connected components
algorithm detecting that this switch is now not connected to the network
anymore:

```
INFO:…: QueryId(0) | Disconnecting randomly chosen switch #149
INFO:…: QueryId(1) | The are now 2 disconnected partitions in the graph!
```

Note that the simulated fault is only temporary. After 10 seconds, the
switch will be reconnected with its peers:

```
INFO:…: QueryId(1) | All nodes in the graph are now connected.
```

