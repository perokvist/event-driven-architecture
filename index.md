---
layout: default
---

# Event driven architecture

<em class="sub-heading">-</em>

By Per Ökvist ([@per_okvist](https://twitter.com/per_okvist/))<br/>
Please give feedback and report issues on the [GitHub repository](https://github.com/perokvist/event-driven-architecture/).

## Introduction
---

In this article we'll explore EDA mainly through "Event-based State Transfer", see [Martin Fowler's event patterns](https://martinfowler.com/videos.html#many-meanings-event).
In the examples services and compontents will collaborate through events. The service and component definitions we're using are as follows.

## Service / Component

![Service definition](assets/service.png)

[Source of inspiration](http://media.abdullin.com/blog/2015/2015-03-18-edd-eBay-Barcelona.pdf#page=23)

Each service can react to events, handle request/response and emit events.
Requests can be commands or queries. Responses can be only status codes or results, the request/response could by synchronous or asynchronous, eventual consistent or not. The definition stays the same and these options are implementation details.

## Logs

This article will also look at integration between services using logs.

![Log infrastructure options](assets/logs.png)

Logging is characterized as **append only**, **ordered events** (in partition) and **offsets**, and could easily be compared to a file(s). Pushing position and acknowledging/"checkpointing" to the clients, removing broker concepts, centralized subscriptions and deadletter etc.

Using logs as integration or/and as a form of persistant model introduces new patterns when working with events. These options are going to be explored further in this article.

![integration through log](assets/service_log_integration.png)

## 1. 2PC

A common challenge when emitting changes/events is to drive the local state and publish the events in a "safe" manner. If the service updates local state first in one transaction then writes the events to a log or a broker in another transaction, you have to deal with two transactions. So you need a [two-phase commit](https://en.wikipedia.org/wiki/Two-phase_commit_protocol).
Not dealing with this could mean loosing data/events.

![Two-phase commit](assets/2pc.png)

When both transactions complete we can return 200 OK to clients. If this has business implications we would like to remove this complexity.

## 2. Polling

Another variant is to poll the source of events. This allows local state to be written, but the producing service doesn't publish the events. Instead the consumer polls based on time or offset. The consumer keeps track of the time or offset that is last polled from the producer. This variant introduces coupling between producer and consumer, as well as potential global offset for the producer, and increased traffic.

![Polling](assets/polling.png)

## 3. Single publisher

This variant has a lot in common with polling. Here a publisher does the polling and then publishes the events. But due to the fact that the publisher needs to keep track of the offset and publish the events, two-phase commit challenges come back and the implication is often that the same event could be published twice.


![function or lambda as polling publisher](assets/publisher.png)

## 4. Log subscription

In this variant we only write to the log, and then reads the log to update local state. This way we only have one transaction when writing to the logs.
This have similarities with using logs to avoid dual writes as detailed in [kafka dual writes](https://www.confluent.io/blog/using-logs-to-build-a-solid-data-infrastructure-or-why-dual-writes-are-a-bad-idea/).


![integration through log](assets/service_log_integration.png)

This variant also has a lot of tweak how we could respond to command from http requests. We could hide the async implementation or embrace it.


![response options](assets/response_options.png)

### 4.1 
We could expose our async implementation, by responding 204 when we receive the command.
### 4.2
We could return 200 ok, when we have written to the logs.
### 4.3 
We could wait and return 200 ok when we have updated the local state. This also gives us an option to return the result.

### Concurrency
Responding to the client is one thing, but what about multiple commands targeting the same instance? This also is an implementation detail, if the use cases need it, we could use locks. In the response scenario above we use the same locks to determine when we complete.

## 5. The truth is the log. The database is a cache

A quote from Pat Helland's paper ["Immutability Changes Everything"](http://cidrdb.org/cidr2015/Papers/CIDR15_Paper16.pdf) and exemplified in ie. ["From Microliths To Microsystems"](https://www.slideshare.net/jboner/from-microliths-to-microsystems).

The final variant is to treat the log as our database. This could be done using infinite retention (Kafka only) or some form of snapshoting, preferably Log Compaction (Kafka only).

Then all other representations of the current state are views/projections or cache of the current state.

### Idempotency

Due to the fact that events are ["Event-based State Transfer"](https://martinfowler.com/videos.html#many-meanings-event) events, not [CRDT](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) events and common infrastructure is at-least-once delivery, idempotency on the consumer side becomes important as do order.

Some of the variants above also push "problems" to the consumer side, like updating local state and manage checkpointing. The could also introduce 2PC, but in worse case handle the same event more than once.

## Event sourcing

If local state is persisted, how is an implementation detail. Keeping a stream of events per instance (aggregate) as the source of state is often refered to as Event Sourcing. Often tied to using Domain driven design (DDD). DDD is not required for the patterns above but might be a good fit, same applies to event sourcing.

<script src="https://gist.github.com/gregoryyoung/a3e69ed58ae066b91f1b.js"></script>
Outside of DDD, this could be refered to as a journal or log. The terms journal, log and streams are found in both eventsourcing and stream processing (logs).

----

## Implementation

### Tactical patterns

When practicing DDD, tacitacal patterns could be utilized in implementation in the solution space. (problem space / solution space).
The patterns could be used in our service/component defention.

![Service defentition](assets/service.png)

In addition to the basic patterns, there are a few building blocks that could also aid us.

| Name  | Formula |
| ------------- | ------------- |
| Application Service  | command -> unit  |
| Application Service  | command -> event*  |
| Enricher  | event -> event  |
| Policy  | event -> event  |
| Receptor  | event -> command  |




## Code

### Command Dispatcher

<script src="https://gist.github.com/gregoryyoung/7677671.js"></script>


TODO

Dispatcher
https://gist.github.com/yevhen/7682490
8-lines of code
Dispatcher
https://gist.github.com/yevhen/7682490
8-lines of code
