# 01 — System context

**Scope:** MoveX and its human actors/external dependencies. Internal service boundaries are intentionally hidden at this level.

![MoveX system context](01-context-diagram.png)

```mermaid
flowchart LR
    rider[Rider]
    driver[Driver]
    ops[Operations and support staff]
    mx[MoveX ride platform]
    maps[Mapping and routing provider]
    identity[Identity-verification sources]
    payments[Payment processor]
    messaging[Notification gateways]
    responder[Market-approved safety-response destination<br/>not yet selected]

    rider -->|request, follow, pay, share trip| mx
    driver -->|onboard, accept, update trip/location| mx
    ops -->|investigate and administer enabled markets| mx
    mx -->|geocode and route estimates| maps
    mx -->|verification checks| identity
    mx -->|payment attempts and outcomes| payments
    mx -->|user notifications| messaging
    mx -->|emergency action status/routing| responder
```

The response destination is a dependency placeholder, not a claim that emergency services or a staffed response operation is available. See [the architecture overview](README.md#assumptions-and-open-decisions).
