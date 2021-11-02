---
layout: default
---

## Modules

![Trends 2021 04](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/architecture-trends-2021/en/resources/1Architecture-2021-1618415938711.jpg)

Even if targeting a distributed architecture or a "modular monolith" - the notion of the module is key.
A common starting point for a "modular monolith" is some version of feature slicing. Where a feature is either a command or query. This notion of a thin slice maps to the early days of "micro frontends" where a feature slice goes from a web component to a FaaS function.

![Feature slices](/assets/feature_slices.png)

Commonly these feature slices utlize a larger model, where aspects like business rules and transaction bounderies doesn't fit pure use-case slicing. The need for a other boundery starts emerging. The entry point might look familiar through commands and queries, with addition to support eventing. The module protectes a model through a boundery facade with signatures for handling commands, queries, eventing (incoming/outgoing). 

![Micro frontend slice](/assets/slice_micro_frontend.png)

![Slices module](/assets/feature_slices_module.png)

With this notion of a module, being part of a monolith or a distributed system start to look the same.

![Module facade](/assets/module_facade.png)

>*"The modular monolith’s beauty is that the software architecture looks surprisingly similar to the microservices architecture you might draw if you were so inclined."

The usage of such approach lends itself quite good to having a "pure domain" or "functional core". Where we could keep infrastructure on the outside and adapt use cases to different protocols.

https://www.youtube.com/watch?v=SAjsP2igBNk&list=WL

But the hardest part still remains - how to explore, find, define and evolve the system boundaries using heuristics valuable for the product/system.







