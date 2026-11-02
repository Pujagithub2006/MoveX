# 04 — Repeatable multi-region deployment pattern

**Scope:** A cloud-neutral logical placement pattern. Cell count, named regions, failover behavior, replication, and residency are not selected.

![MoveX multi-region deployment pattern](04-deployment-diagram.png)

```mermaid
flowchart TB
    clients[Rider, driver, and operations clients]
    routing[Global traffic routing<br/>market activation gate]

    subgraph cellA[Enabled market region cell A]
        ingressA[Regional ingress and edge]
        servicesA[Stateless MoveX service runtime]
        busA[Regional event backbone]
        dbA[Durable service-owned stores]
        redisA[Ephemeral location/offer state]
        telemetryA[Region-scoped telemetry]
        ingressA --> servicesA
        servicesA --> busA
        servicesA --> dbA
        servicesA --> redisA
        servicesA --> telemetryA
        busA --> telemetryA
    end

    subgraph cellN[Enabled market region cell N — repeatable pattern]
        ingressN[Regional ingress and edge]
        servicesN[Stateless MoveX service runtime]
        busN[Regional event backbone]
        dbN[Durable service-owned stores]
        redisN[Ephemeral location/offer state]
        telemetryN[Region-scoped telemetry]
        ingressN --> servicesN
        servicesN --> busN
        servicesN --> dbN
        servicesN --> redisN
        servicesN --> telemetryN
        busN --> telemetryN
    end

    providers[Market-appropriate external providers]
    clients --> routing
    routing --> ingressA
    routing --> ingressN
    servicesA --> providers
    servicesN --> providers
```

The cells are a repeatable pattern, not a deployment count or a guarantee of cross-region failover. The drawing intentionally does not imply cross-region replication: data placement, residency, and recovery policy require market/legal and NFR decisions. See [the architecture overview](README.md#assumptions-and-open-decisions).
