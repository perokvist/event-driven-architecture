## Implementation

### Tactical patterns

When practicing DDD, tactical patterns could be utilized in implementation in the solution space. (problem space / solution space). These patterns could be used in our service/component definition.

![Service defentition](assets/service.png)

In addition to the basic patterns, there are a few building blocks that could also aid us.

| Name  | Formula |
| ------------- | ------------- |
| Application Service  | command -> unit  |
| Application Service  | command -> event*  |
| Enricher  | event -> event  |
| Policy  | event -> event  |
| Receptor  | event -> command  |

Examples as follows.

## Code

Commands and events are central in all examples. In OO some registry for handlers could be handy (or pattern matching available). Commands have one target, events are broadcasted. In the **EventProcessor** we use locks as mentioned in the variants above.

### Command Dispatcher

<script src="https://gist.github.com/gregoryyoung/7677671.js"></script>
[8 lines of code](https://www.infoq.com/presentations/8-lines-code-refactoring) (video)
<script src="https://gist.github.com/perokvist/2310c6f7a2bc2c16b86332903e369899.js"></script>

### EventProcessor

To broadcast events. Some consumer that don't need the lock, might want to another way of broadcating or handling IO than Task.WhenAll.

<script src="https://gist.github.com/perokvist/ef866f886df25d93ef7e9cca283456c0.js"></script>

### Application service
When dispatching commands to application services, a util could come in handy to support some of the variants above. This could be used in Application services for your use cases or in simple scenarios directly in the dispatcher.
<script src="https://gist.github.com/perokvist/409f474559f44657e8d2cdf19a53b94d.js"></script>

### Test
If state is based on events, test could be like: 
- Given (past events)
- When (action/command)
- Then (events)

See [event driven verification](https://abdullin.com/sku-vault/event-driven-verification/).

### Service/Component/Module -  Given, When, Then

Our scenario of focus is a service that owns state, with possible constraints implemented to protect its state from illigal invariants.

Consumers that only own projection(s) of these events, does not have the same complexety.

In the examples above *When* is handled by the dispatcher, *Then* (event returned by the aggregate) is handled by the EventProcessor (in many cases).

Our goal is to write events in "one" transaction (to all consumers), and use aggragetes to scope and aid concurrency.

We use locks in the example to illustrate how *when* could be used through an api. When the service is both a producer and a consumer (changes state protected by constraints - handels both *when* and *then*) it needs to share the same lock reference. This way an aggregate only handles one command at a time with the latest state.

The producer is also a consumer due to that we want to write our events (in this case to the log) in one transaction, so all consumers (including the producer) recieves the events.
The producer recieves the events at the "same time" as consumers. In our example locks are used to aid both concurrency and options for the api to respond.

This gives us quite alot of implementation options.

#### Options

#### Push vs Pull

The publisher could either write/publish events to a log or/and an eventstore stream. Consumers could then get events trough the log, eventstore or service api.

When the consumers gets events through the log, integration is done via the log and no other coupling is introduced (besides infrastructure and schema).

The producer could also expose its events through ex Atom feed, the consumer would then pull changes from the producer. This would introduce coupling between the services, but ease implementation code wise.

The consumer could also get published events from the event stream from its store, tying integration to the "database".

Databases like [Eventstore](https://geteventstore.com/blog/20130306/getting-started-part-3-subscriptions/) or [CosmosDB](https://docs.microsoft.com/en-us/azure/cosmos-db/change-feed) offers subscriptions to changes. These could also be used to create a single publisher.

#### Tests

When creating a service/component it could aid tests to ease the use of setup to use given, when, then apis. 

##### Actors

Actors is used for concurrency guarantee (and scaling), incoming commands are queued (inbox) per actor instance, allowing one command to change the instance state at a time. Using actors you still face the same challanges in writing (and publishing) state/events to its persistance (and log) in a transactional way. Some of the challanges could be solved by the actor framework.

Service fabric offers an actor model out of the box and does also support Orleans and other Actor frameworks.

##### Service fabric
Service fabric let you work with an instance queue and its state in a single transaction (with replication support) and loadbalancer with partion support.

So even if you don't use actors service fabric has a lot of features that suite our scenarios. 
But if you not only use its persitance model, some of the challages in this article still apply.

#### AspNetCore 2.0

When creating components in aspnetcoremvc 2.0 that either subsrcibes to a log, stream subscription or polls a feed [IHostedService](https://www.stevejgordon.co.uk/asp-net-core-2-ihostedservice) provides hosting an http api an creating background work with the same lifetime as the api host. 

#### Azure EventGrid

[EventGrid](https://blog.tomkerkhove.be/2017/08/22/exploring-azure-event-grid/) is a event delivery and routing service. Using EventGrid introdues a new service that becomes the integration point. EventGrid pushes Events to functions, logic app or to web hooks. The grid is Topic based and offers at-least-once delivery but retry and perstance time is 24h.

An app could publish event to eventgrid through http, or builtin in hooks from event publishers like from Event hubs could be used.

This gives us alot of options, events could be written to a log, but routed to web hooks, for easier apis for consumers.

Two services could integrate only trough eventgrid, a possibility to even only use eventgrid to get on transaction is possible where the producing service pushes event to the grid and also is a web hook consumer.

#### Serverless 

Many of these scenarios is about a service/compontent that is both a producer and a consumer, that want a single transaction before events are in some way persisted. Locks enables the api to know when the events are persisted in a log stream or when it's also retrived and persisted in the compontent eventstore.

If you have another scenario where you poll or in otherway subscribe to events thats already persisted or just a service wiht projections, you don't need to share a locks reference in the same process. Then serverless independent functions for publisher and consumers could be use with ease.
If a function is for an interaction (command) you still have to choose a concurrecy option.





