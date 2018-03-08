---
title: Navigating the source code
nav-index: 6
category: Strymon Core
---

The Strymon open source edition currently contains of a single repository
called [`strymon-core`](https://github.com/strymon-system/strymon-core),
containing the publish-subscribe mechanism as well as a simple
example application.

## Directory structure

 - `apps/`

   The example applications shipped with Strymon core. Please refer to
   [how to run an example](running-the-example) for more information about
   them.

 - `src/`

   This folder contains the source code for the crates which form the core of
   the Strymon publish-subscribe mechanism and execution run-time.

    - **[`strymon_cli`]**: This binary crate contains the implementation of the
      **`strymon`** command-line interface.

    - **[`strymon_job`]**: Client library for writing custom Strymon jobs. This
      crate also implements the publish-subscribe protocol.

    - **[`strymon_coordinator`]**: The implementation and internal APIs of the
       coordinator and catalog.

    - **[`strymon_executor`]**: The default executor implementation and job
       spawning logic for native executables.

    - **[`strymon_rpc`]**: The message types used for remote-procedure calls
       between the different Strymon components.

    - **[`strymon_model`]**: Common data type definitions, used mainly in the
      catalog.

    - **[`strymon_communication`]**: Low-level networking code used for
       communication between the different components of Strymon.

 - `bin/`

   The command-line utility for interacting with the Strymon cluster. Their
   usage is documented in [here](command-line-interface).

 - `conf/`

   Folder for configuration files. At present, contains a default host file
   for running the executors. The `conf/executors` file shall contain a
   single host name per line. Note that the scripts currently do not
   support running more than one executor per machine.

  - `logs/`

    The default destination for log files created by the coordinator and
    executor instances.

  - `jobs/`

    A temporary working directory for storing the artifacts of running jobs.
    Each job process gets its own directory:
    `jobs/<submission-timestamp>_<job-id>/<process-index>/`

[`strymon_cli`]: command-line-interface
[`strymon_job`]: https://strymon-system.github.io/strymon-core/strymon_job/index.html
[`strymon_coordinator`]: https://strymon-system.github.io/strymon-core/strymon_coordinator/index.html
[`strymon_executor`]: https://strymon-system.github.io/strymon-core/strymon_executor/index.html
[`strymon_rpc`]: https://strymon-system.github.io/strymon-core/strymon_rpc/index.html
[`strymon_model`]: https://strymon-system.github.io/strymon-core/strymon_model/index.html
[`strymon_communication`]: https://strymon-system.github.io/strymon-core/strymon_communication/index.html
