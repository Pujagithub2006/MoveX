# 03 — Core ride workflow components

**Scope:** A logical decomposition of the request-to-trip-to-payment workflow and its safety/notification side effects. APIs, event schemas, database schemas, and matching algorithms are deferred.

![MoveX core ride workflow components](03-component-diagram.png)

```mermaid
flowchart LR
    clients[Rider and driver clients]
    edge[Edge authentication and routing]
    quote[Quote and pricing]
    eligibility[Market and driver eligibility]
    location[Location ingestion and lookup]
    match[Match coordinator and offer lifecycle]
    ride[Ride lifecycle coordinator]
    rideStore[Ride-owned durable state]
    hot[Ephemeral location and offer index]
    bus[Lifecycle event backbone]
    payment[Payment coordinator]
    safety[Safety action coordinator]
    notify[Notification dispatcher]
    providers[External adapters<br/>maps, identity, payments,<br/>messaging, safety response]
    ops[Audited operations/support view]

    clients --> edge
    edge --> quote
    edge --> ride
    edge --> location
    edge --> safety
    edge --> ops
    quote --> providers
    ride --> eligibility
    ride --> match
    location --> hot
    match --> hot
    match --> eligibility
    ride <--> rideStore
    ride --> payment
    ride --> bus
    payment --> providers
    payment --> bus
    safety --> providers
    safety --> bus
    bus --> notify
    bus --> ops
    notify --> providers
```

The ride coordinator owns trip-state decisions. Matching and payment report outcomes to that lifecycle without taking ownership of the trip record. Location/offer indexes are ephemeral; durable ride and payment outcomes remain with their domain owners. See [the architecture overview](README.md#logical-service-responsibilities).
