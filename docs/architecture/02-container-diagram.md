# 02 — Logical containers

**Scope:** Client applications, the MoveX service capabilities, shared communication/data roles, and external dependencies. Boxes are logical ownership boundaries, not a commitment to one process per box.

![MoveX logical containers](02-container-diagram.png)

```mermaid
flowchart LR
    clients[Rider app, Driver app,<br/>Operations console]
    edge[Regional edge and API gateway]

    subgraph domain[MoveX domain capabilities]
        accounts[Identity and accounts]
        eligibility[Driver eligibility and<br/>market readiness]
        pricing[Quote and pricing]
        ride[Ride orchestration]
        location[Location]
        matching[Matching]
        payment[Payment]
        safety[Safety]
        notify[Notifications]
        ops[Operations and support]
    end

    events[Event backbone<br/>Kafka candidate]
    durable[Service-owned durable stores<br/>PostgreSQL candidate]
    hot[Ephemeral geo and offer state<br/>Redis candidate]
    providers[External adapters: maps, verification,<br/>payment, notification, safety response]

    clients --> edge
    edge --> accounts
    edge --> eligibility
    edge --> pricing
    edge --> ride
    edge --> location
    edge --> matching
    edge --> payment
    edge --> safety
    edge --> ops
    accounts --> eligibility
    eligibility --> matching
    pricing --> ride
    ride <--> matching
    location --> matching
    location --> hot
    matching --> hot
    ride --> events
    matching --> events
    payment --> events
    safety --> events
    events --> notify
    events --> ops
    accounts --> durable
    eligibility --> durable
    pricing --> durable
    ride --> durable
    payment --> durable
    safety --> durable
    ops --> durable
    pricing --> providers
    eligibility --> providers
    payment --> providers
    notify --> providers
    safety --> providers
```

The durable-store node represents separate ownership, not a shared database. The event backbone is for lifecycle fan-out and asynchronous work; it is not the synchronous request path. Technology names are candidates only. See [the architecture overview](README.md#data-and-communication-roles).
