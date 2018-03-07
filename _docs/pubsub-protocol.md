---
title: "Pub-Sub Protocol Internals"
nav-index: 5
category: Strymon Core
---

This document describes the internal protocol of Strymon's built-in
publish-subscribe mechanism for streams. The implementation of this protocol,
including documentation for users, is part of the [`strymon_job`][1] library.

## Design Goals

In Strymon's publish-subscribe mechanism, publishers are mostly stateless:
If there are no connected subscribers, any published messages are dropped, as
it would be too expensive to buffer all produced messages.
However, subscribers might connect at any point in time and still need to be
able to reason about the current state of a Timely stream source.

Notably, a subscriber must be able to reason about the advancment of logical
time ("epochs") in the stream source. It must be informed about both,
timestamps which it will still be able to observe, as well as times which have
already passed.

As tuples with the same timestamps share the same *happend-before* relationship,
so a subscriber should only ever see either *all* tuples belonging with a
certain timestamp, or *none* at all. We define this as *atomicity of epochs*.
This property is important for Strymon applications such as
[SnailTrail](https://github.com/strymon-system/snailtrail).

## Terminology

A Timely stream is a stream of (`T`, `D`) tuples where `T` refers to some
partially ordered logical timestamp (an "epoch") and `D` is some unspecified
data item.

We introduce the terminology of `latent`, `active` and `completed` epoch states,
i.e. an epoch `T` can only be in one of the three states (as observed by
the publisher).

                        +---------------+
                        |    latent     | --------------------
                        +---------------+                    |
                                |                            |
               [first tuple with epoch `T` observed]         |
                                |                            |
                                v                            |
                        +---------------+                    |
                        |    active     |             [skipped epoch]
                        +---------------+                    |
                                |                            |
               [last tuple with epoch `T` observed]          |
                                |                            |
                                v                            |
                        +---------------+                    |
                        |   completed   | <-------------------
                        +---------------+


An epoch `T` is `latent`, if it has never been observed. Once a tuple with a
certain epoch has been observed on the stream, the corresponding epoch is
considered `active`. Once the last tuple for a given epoch has been delivered,
the epoch is considered `completed`. An epoch can be skipped if upstream
operators release its capability for it without ever producing any tuples.

The frontier tracked by Timely allows us to infer `completed` epochs: If a
epoch `T` is strictly smaller than any `S` found in the frontier, then the
epoch is `completed`. In other words: Timely explicitly tells us about
completed epochs. The start of an epoch (the transition to the `active` state)
however is implied implicitly with the delivery of the first tuple of that
epoch.

## Design

Atomicity of epochs can easily be guarantee if we ensure that a subscriber
only ever receives tuples which were in the `latent` state at the time when it
started listening to the publisher: A latent epoch has not yet produced any
tuples, thus the subscriber could not have missed any.

The simplest approach would be to keep track of all `active` epochs at the time
of subscription and use this set as a filter, i.e. only forward tuples to the
subscriber which are not in this set.

While this would work (and the size of this set should not an issue in
practice, as the number of `active` epochs is typically not too high), it might
not be intuitive. Consider the following example with totally ordered integers
as timestamps:

    States at subscription time:

      completed: [0, 1, 2]
      active: [3, 5]
      latent: [4, 6, 7, 8..]

The subscriber in this example would see epoch `4`, followed by epoch `6`,`7`,
`8`.., with a hole at epoch `5`. While atomicity of epochs is preserved, it issue
more intuitive to avoid this hole in observed epochs and not deliver epoch
`4` to the subscriber at all.

The proposed approach thus is to use the set of *maximal elements* in `active`
as a filter for delivered epochs. This set of *maximal `active`
elements* allows us to approximate the `latent` set. Note that in a total order,
it is a singleton set (in the example: [`5`]), in a partial order however it
forms an antichain.

This is similar to how Timely keeps track of `completed` events, namely
its frontier is the antichain of *minimal elements* in the `active` set,
allowing it to test for membership of the `completed` set.

We define these two antichains as the lower and upper frontier:

  - `lower frontier`: antichain of *minimal* elements in `active`
  - `upper frontier`: antichain of *maximal* elements in `active`

Taking a snapshot of the `upper frontier` at the time of subscription, we can
use it as a filter on arriving tuples, forwarding only elements which are
strictly larger than any element in the `upper frontier`. This ensures that
the subscriber only ever observes epochs which were `latent` at the time of
subscription, thus ensuring atomicity of epochs.

Once the *current* `lower frontier` dominates the *snapshot* of the
`upper frontier`, the filter can be disabled.

### Implementation

The publisher maintains the antichains of the `lower` and `upper` frontiers.
The `lower` frontier is maintained from **progress** tracking traffic using
Timely's existing mechanisms. The `upper` frontier however has to be inferred
by inspecting the epochs of arriving **data** messages, as new epochs are opened
implicitly once they are observed.

In the current implementation, the filtering is performed *at the subscriber*.
The publisher is responsible to create an initial snapshot of the two frontiers
and send it downstream. After the initial snapshot, it sends all unfiltered
data tuples to all subscribers. Additionally, it also sends updates to the
`lower frontier` downstream. This design allows allows for more flexibility:

  - The subscriber can decide apply a even more coarse grained filter
    than the initial `upper frontier` to the stream. This for example is
    necessary if a subscriber reads from multiple partitions.
  - The subscriber implementation might decide to not apply a filter at all.
    While this breaks epoch atomicity, some applications do not require it.

Updates to the lower frontier are sent downstream because the subscriber might
maintain a copy of the *current* `lower frontier` in order to derive the
capability set for downstream operators.

#### Protocol Messages

The protocol conceptually consists of three messages:

```rust
enum Message<T, D> {
    // the first message with both frontiers, only sent once
    InitialFrontierSnapshot { lower: Vec<T>, upper: Vec<T> },
    // updates to the lower frontier,
    // each tuple is either (T, -1) or (T, +1)
    LowerFrontierUpdate { updates: Vec<(T, i32)> },
    // a normal batch of (T, D) messages
    DataMessage { time: T, data: Vec<D> },
}
```

The initial snapshot is always sent as the first message to any newly connected
subscriber, after that only `DataMessage` or `LowerFrontierUpdate` messages
will be observed.

The current implementation builds on top of [`strymon_communication`][2]'s
message type. The `time` and `data` fields are put into separate multi-part
slots. This allows us to drop filtered message without decoding the data part.

#### Publisher Logic

A publisher receives Timely events (progress tracking or data messages) from
a single upstream Timely stream partition and it maintains a queue of outgoing
pub-sub protocol messages for each connected subscriber (initially empty).
The publisher starts with an empty `upper frontier` and a `lower` frontier
containing a single bottom element, modeling the initial capability upstream.

The publisher's event loop handles incoming events as follows:

  - *A Timely progress tracking message (`Vec<T, i64>`) arrives:*
    1. Update the local `lower frontier` antichain.
    2. Broadcast any resulting changes to the frontier to all connected
       subscribers (`Message::LowerFrontierUpdate`).
  - *A Timely data batch  `(T, Vec<D>)` arrives:*
    1. Update the `upper frontier` antichain.
    2. Broadcast the batch to all connected subscribers (`Message::DataMessage`).
  - *A new subscriber connects:*
    1. Take a snapshot of the current `lower` and `upper` frontier.
    2. Send this snapshot the the new subscriber
       (`Message::InitialFrontierSnapshot`)
    3. Add the subscriber to the list of connected subscribers.
  - *A subscriber disconnects:*
    1. Remove the subscriber from the list of connected subscribers.

#### Subscriber Logic

After consulting the catalog about the address of a publisher in the catalog,
a subscriber connects directly to a publisher and waits for the
`InitialFrontierSnapshot` message.

Once the initial snapshot of the upper and lower frontier have been received,
it enters the following event loop:

  - *A `DataMessage { time: T, data: Vec<D> }` arrives*:
      1. if `T` is less or equal than any element of the initial snapshot
        of the `upper frontier`, drop the data message. Otherwise, decode
        the `data` part and yield the decoded message to the consumer.
  - *A `LowerFrontierUpdate { updates: Vec<(T, i32)> }` message arrives*:
      1. Update local `lower frontier` antichain from `updates`. This frontier
         is exposed to consumers and can be used to downgrade capabilities.
      2. Remove any elements `S < T` from the initial `upper frontier`, as they
         are not needed for filtering anymore.

Subscribers in Strymon can subscribe to multiple partitions. In this case,
it will first wait for all initial upper frontier snapshots and merge then into
a maximal antichain for the filter. This ensures that epoch atomicity is
preserved across all partitions. The lower frontier is also merged accordingly
into a minimal antichain.

[1]: https://strymon-system.github.io/strymon-core/strymon_job/index.html
[2]: https://strymon-system.github.io/strymon-core/strymon_communication/index.html
