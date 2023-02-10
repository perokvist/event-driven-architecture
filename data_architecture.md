---
layout: default
---

# Bridging the gap between transaction and analytics data with events - (data+architecture)

Team scope and its software boundaries has always been an important aspect of architecture. With more aspects expected to be covered by cross functional teams, and a more distributed nature of many systems, teams and its boundaries are key, forming an organization to form the right conditions. Team topologies is an approach that could aid in forming technology organizations. 

But what happens when we add data to the mix ?

As data becomes even more important, we see that autonomous teams (value stream teams) need data as both feedback and as an enabler. This leads to keeping team/domain data close, within the domain. Here we see the relation between Domain driven design (DDD) and Data Mesh architecture. In moving complex concepts close, cross cutting concern and the need for more cross functional expertise grows, we see platform team(s) playing an important role, complemented with enabling teams in some cases. 

InfoQ coins “[data+architecture](https://www.infoq.com/articles/architecture-trends-2022/)” to describe software architectures incorporating data.

"Data plus architecture is the idea that, more frequently, software architecture is adapting to consider data. This holistically includes data quality, data pipelines, and traceability to understand how data influenced decisions and AI models”

In this article we’ll take a glance at how events could play a role in different aspects of data.

## Data
We could use three different categories to divide common data usage; 

**Logs**, application logs and custom trace(s) and events.

**Transactional data** (operational), model(s) for the data the system protects from invariants through constraints, i.e. the data you change from system interactions through - commands. The same model is commonly used  to fulfill queries with view models.  

**Analytics data**, model(s) built for supporting analytics.

Entities/documents are common in **transactional** models and **analytics**, and events are common in **logs**, **transactional** and **analytics data**. This makes events an interesting part in bridging the gap between **transactional** and **analytics data**.

On the road to data analytics within the domain, teams often start with using logs for initial analytics, by sending custom trace(s) and events to their logging infrastructure.  Most log analytic services have capable tools for simple analytics, but with retention of data as a common case.

![kinds of data](assets/kinds_of_data.png)

No alt text provided for this image
All data could act as feedback for teams, transactional data and click streams could be exposed in BI as feedback. Building out analytics is a continuation of investment in data. Output from analytics could also end up supporting applications and surface as views to users.

## Events
Events could be utilized in each category, and introduce synergies, between transactional and analytics data.

In Log analytics services, there are different kinds of events, sending both “UI Click / page views” events (telemetry) and custom log events along with other application logs.

System Side effects in the transactional model as events, formulated in past tense (ApplicationApproved, OrderShipped, GameEnded),  could be implemented in many ways. 

Examples;

- Publishing events after the state of the system has changed (persisted)
    - Completeness guarantee (can you trust the events data, since events and state are separate ?)
    - Publishing has 2PC(two phase commit), publishing guarantees / outbox challenges
- Derive state by applying events to state, then persist, before publishing the events.
    - Publishing could have 2PC / outbox challenges
    - Option - snapshot on every change - never evolve state from history
- Persist events and derive (evolve) system state from prior events (event sourcing). 

Commonly with support for Tailing/subscription of DB changes
Events could allow you to derive other models optimized for different cases, like read models (view models optimized for queries) (CQRS).

## Event Types
There are many kinds of events, to simplify we could differentiate Domain Events from Integration events. Domain events are granular and “internal” to the publishing boundary, while integration events are “larger” events for inter boundary communication. So when publishing events this could mean - for storing (data), internal communication, or for building integration events to publish for inter boundary communication. 

For data analytics scenarios, both types of events could be of interest, depending on the case. As in the transactional model, these events also could be  used to derive model(s) for analytics.

## Data mesh
“The domain ownership principle mandates the domain teams to take responsibility for their data. According to this principle, analytical data should be composed around domains, similar to the team boundaries aligning with the system’s bounded context. Following the domain-driven distributed architecture, analytical and operational data ownership is moved to the domain teams, away from the central data team.” 

- https://www.datamesh-architecture.com/ 

In moving ownership of data to a team/domain level, the influence on architecture is greater, and how transactional analytics data could collaborate. Having that ownership might improve “data quality, data pipelines, and traceability”.  But building a self-serve data product, that also others could use (mesh), support from a “platform team”  could be needed for a team to handle the complexity.

Data Mesh could be seen in itself as an example of data+architecture with the influence of domain driven design on the perspective of data.

Data meshes are not widely adopted, but illustrate the influences and relation with DDD. 

## Closing
Introducing events as side effects of interactions (commands) in the transactional model, and storing/exporting them could give a starting point for analysing data. This introduces other options than exporting a snapshot of the transactional data on change, or as common only looking at the current state, using the transactional model. 

Other strategies include change data capture (CDC) to be able to extract change records from a source, similar to tailing an event store (when records are events), these records could leak data models, interpretations and lack intent of change.  Then there are classic ETL where transformation of data could happen in transit. 

Some types of events are often introduced to reach reactive async collaboration, a stepping stone into event driven architecture.

The event viewpoint also shines in collaborative design methods like event storming and event modelling, adding quality to architecture and value of data.

It will be interesting to follow how data+architecture moves in the next InfoQ trend report, along with related topics.
