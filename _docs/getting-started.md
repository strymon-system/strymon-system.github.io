---
layout: docs
title: Getting started
nav-index: 1
---

### Get Strymon

You can download any release of Strymon from the [release archive](/download) or
fetch the most recent version directly using `git`:

```terminal
$ git clone https://github.com/strymon-system/strymon-core.git
Cloning into 'strymon-core'...
…
Resolving deltas: 100% (1067/1067), done.
$ cd strymon-core
```

You will need the nightly version of the [Rust](https://www.rust-lang.org/en-US/)
compiler and the [Cargo](https://crates.io/) package manager to build the
Strymon source code.

```terminal
$ cargo build --all --release
```

This step will take a while, as this command builds everything: The Strymon core
run-time, the provided example programs, and essential dependencies such as
[Timely and Differential Dataflow](https://github.com/frankmcsherry/timely-dataflow).

> *Note*: The easiest way to install the nightly version of the Rust compiler
> and Cargo is using [rustup.rs](https://rustup.rs/). You can assign a directory override
> for the nightly version by running the following command
> inside the `strymon-core` project folder:
> ```terminal
> $ rustup override set nightly
> ```

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

> *Note*: Stopping the executor will not terminate any runnig jobs. This will
> change in the future. For now you can terminate them manually as follows:
> ```terminal
> $ pkill -f timely_query
> ```
