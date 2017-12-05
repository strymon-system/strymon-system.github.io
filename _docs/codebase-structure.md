---
layout: docs
title: Navigating the source code
nav-index: 5
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

 - `bin/`

   The command-line utility for interacting with the Strymon cluster. Their
   usage is documented in [here](command-line-interface).

 - `conf/`

   Folder for configuration files. At present, contains a default host file
   for running the executors. The `conf/executors` file shall contain a
   single host name per line. Note that the scripts currently do not 
   support running more than one executor per machine.

 - `src/`

   This folder contians the source code for the crates which form the core of
   the Strymon publish-subscribe mechanism and execution run-time.

   - `src/strymon_runtime/`

      This crate contains the bulk of the execution run-time, the
      implementation of the `strymon` command-line utility and the client
      library used for writing Strymon jobs using Timely Dataflow.

      > **Note**: Both the internal organization and API of this crate is
      > currently not considered stable. Strymon jobs are required to use the
      > API in the `strymon_runtime::query` module to interact with the system.

   - `src/strymon_communication/`

     The networking code and protocol used for communication between the
     different components of Strymon.

      > **Note**: The API of this crate is currently not considered stable.

   - `src/strymon_rpc/`

      This library defines the types used in the remote procedure call protocol
      between the different components.

   - `src/strymon_model/`

      Type definitions of the relations stored in the catalog.

