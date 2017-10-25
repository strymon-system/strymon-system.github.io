---
layout: docs
title: Getting started
nav-index: 1
---

```shell
# compile everything (this can easily takes up to 20 mins!)
cargo build --all --release
# start a coordinator and the executors listed in `conf/executors`
./bin/start-strymon.sh
# query the system status
./bin/strymon status
# stop the coordinator and the executors (note, currelty does not stop queries)
./bin/stop-strymon.sh
```  

