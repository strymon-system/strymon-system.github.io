---
title: Getting started
nav-index: 1
category: Strymon Core
---

### Get Strymon

You can download any release of Strymon from the [release archive](/download) or
fetch the most recent version directly using `git`:

```terminal
$ git clone https://github.com/strymon-system/strymon-core.git
Cloning into 'strymon-core'...
…
$ cd strymon-core
```

You will need a recent version of the [Rust](https://www.rust-lang.org/en-US/)
compiler and the [Cargo](https://crates.io/) package manager to build the
Strymon source code.

```terminal
$ cargo build --all --release
```

This step will take a while, as this command builds everything: The Strymon core
run-time, the provided example programs, and essential dependencies such as
[Timely and Differential Dataflow](https://github.com/frankmcsherry/timely-dataflow).

> *Note*: The easiest way to install the nightly version of the Rust compiler
> and Cargo is using [rustup.rs](https://rustup.rs/).
> As of Stymon Core version 0.2, we support Rust 1.22 or newer. We recommend
> you use the most recent stable version of the Rust compiler.

### Starting a local Strymon cluster

Once you downloaded and built `strymon-core`, run the following commands in
the project folder to spawn a local cluster:

Start a coordinator and the executors listed in `conf/executors`:

```terminal
$ ./bin/start-strymon.sh
```

Ensure that the system is running by inspecting the system catalog:

```terminal
$ ./bin/strymon status
Coordinator: localhost:9189
 Executor 0: host="localhost"
```

For more information about these command-line utilities, please consult their
[documentation](command-line-interface).

> *Note*: on macOS you may be presented with a prompt from the firewall, once
> when starting the cluster and again when running the provided example.  This
> is because the built-in firewall blocks incoming network connections and you
> can safely proceed by selecting "Allow".

### Next step: Try the example programs

The Strymon Core contains a very simple two-stage example of a network topology
generator and a connected components module. You can read more about them
and how to try it out in the next chapter:

<a class="button" href="running-the-example">Click here to continue the tutorial</a>

### Stopping the cluster

To stop the coordinator and the executor running on the local machine,
execute the following command:

```terminal
$ ./bin/stop-strymon.sh
```
