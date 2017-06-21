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