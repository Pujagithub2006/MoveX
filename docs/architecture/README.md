# MoveX High-Level Architecture

**Status:** Phase 1 working design
**Version:** 0.1
**Last updated:** 2026-11-01

## Design summary

MoveX is modeled as a globally reachable, multi-region ride platform composed of repeatable regional service cells. Each cell serves only markets that have passed explicit readiness gates. Within a cell, logically separated product capabilities own their behavior and data; the design does not yet require every capability to be an independently deployed microservice. Synchronous interactions support user-facing decisions, while an event backbone supports durable lifecycle fan-out and asynchronous work.

The design covers immediate, single-rider, point-to-point rides, in-app payment, baseline safety, and the minimal internal operations/support capability selected for Phase 1. It is a future-production design, not an assertion that MoveX is ready for commercial launch.

## Product decisions carried forward

The following are confirmed in Phase 0 and constrain this design:

- The product ambition is worldwide, with India, the United States, Canada, the United Kingdom, Japan, China, South Korea, Germany, and Italy prioritized. City coverage and activation criteria remain open.
- The initial ride model is immediate and point-to-point. Scheduled and pooled rides are excluded.
- In-app payment processing is in scope; methods, currencies, authorization/capture timing, refunds, settlement, and payouts remain open.
- Baseline safety includes identity/driver verification, trip sharing, and an in-trip emergency action. Verification rules and response ownership/coverage remain open.
- Numeric latency, availability, scale, location-freshness, recovery, and retention targets are not approved.
- Operations/support and market administration were open in Phase 0. For this design, a minimal internal capability is included, with role-scoped access and an audit trail; its workflows and staffing are not yet specified.
- The deployment view uses a repeatable multi-region pattern with per-market activation gates. This is a target pattern, not a selected cloud topology or data-residency decision.

See the [product overview](../01-product-overview.md), [functional requirements](../02-functional-requirements.md), [non-functional requirements](../03-non-functional-requirements.md), [scope](../04-scope.md), and [user flows](../05-user-flows.md).

## System boundary

### Inside MoveX

Rider and driver applications, an internal operations/support console, the public service edge, identity and account capabilities, driver eligibility and market readiness, quote/pricing, ride lifecycle, location, matching, payment, safety, notification, and internal operations/support are part of the logical product system.

### External dependencies

MoveX integrates through replaceable adapters with mapping/routing, identity-verification sources, payment processors, notification gateways, and a market-approved safety-response destination. No vendor is selected. An emergency action must communicate its actual delivery/acknowledgement state; the design does not imply a guaranteed response or staffed emergency service.

## Logical service responsibilities

These are candidate ownership boundaries. They may be grouped or separated at deployment time after workload, team, and operational requirements are known.

| Capability | Owns | Deliberately does not decide yet |
|---|---|---|
| Edge/API gateway | Client entry, routing to regional services, transport policy, and coarse abuse controls. | API contracts, specific gateway product, and numeric limits. |
| Identity and accounts | Sign-in, account/profile lifecycle, and role/identity context for rider, driver, and internal users. | Identity provider, age/account policy, and recovery rules. |
| Driver eligibility and market readiness | Driver eligibility state and market-specific readiness/configuration gates. | Verification standards, evidence retention, and exact market rules. |
| Quote and pricing | Validates quote inputs and returns a market-configured fare estimate. | Fare formula, quote validity, currency/methods, and fees. |
| Ride orchestration | Authoritative ride lifecycle and permitted state changes for one rider, one driver, one origin, and one destination. | API shape, schema, state-machine implementation, and cancellation policy. |
| Location | Ingests and serves time-sensitive participant location, with stale/unavailable status visible to consumers. | Update cadence, retention, precision, and freshness target. |
| Matching | Selects eligible nearby drivers, manages offers and matching outcomes, and emits state changes. | Ranking algorithm, radius, offer fan-out, and timeout targets. |
| Payment | Coordinates in-app payment attempts and user-visible outcomes; preserves pending/ambiguous outcomes for recovery. | Provider, payment methods, capture timing, refunds, reconciliation schedule, and driver payouts. |
| Safety | Supports verification-related state, trip sharing, and emergency-action delivery/acknowledgement tracking. | Market checks, response destination, escalation policy, and response-time promise. |
| Notifications | Delivers rider/driver updates for material ride and payment events and reports delivery state where available. | Channels, templates, and delivery guarantees. |
| Operations and support | Provides minimal, audited, role-scoped investigation and market-readiness administration. | Persona matrix, exact tools, data visibility, and staffing/coverage. |

Ratings and a product analytics service are not included as baseline capabilities: neither is established by the Phase 0 requirements. Observability is a platform capability, not a product analytics service. A search index is also omitted until support workflows establish a search requirement.

## Data and communication roles

Technology names below are candidates from the project roadmap, not approved selections.

| Role | High-level use | Candidate / guardrail |
|---|---|---|
| Durable domain state | Each owning capability persists its authoritative records and state transitions. | PostgreSQL is a candidate relational store. Treat ownership as per service; do not infer one shared schema or define schemas in Phase 1. |
| Ephemeral location/offer state | Fast proximity lookup and short-lived dispatch state. | Redis is a candidate. It is rebuildable/expiring state, not the authoritative trip or payment record. |
| Event backbone | Durable fan-out for ride, matching, payment, and safety lifecycle events; asynchronous notification and operational projections. | Kafka is a candidate. Do not route every user-facing read/write through the broker. |
| Search index | Not required by the confirmed product flows. | Elasticsearch is deferred until an approved support/search use case, access policy, and data-retention design exist. |

Use synchronous calls when the user needs a direct decision, such as a quote or a command acknowledgement. Publish lifecycle changes for independent downstream work such as notifications and operational views. Payment timeouts and emergency-action failures must remain explicit states rather than being presented as success.

## Key trade-offs and rationale

1. **Logical service boundaries before deployment boundaries.** The domain responsibilities are explicit, but deploying every capability separately now would add network, ownership, and operational overhead without approved workload or team assumptions. The boundaries allow later extraction where independent scaling, change, or reliability warrants it.
2. **Repeatable regional cells instead of a single global data plane.** Cells provide a path to market-specific activation and reduce dependence on one region. They increase operational complexity and do not by themselves decide cross-region failover, data residency, or consistency. Those remain open.
3. **Events for fan-out, synchronous paths for user decisions.** Event-driven communication supports decoupling and recovery for downstream work, but eventual consistency and duplicate delivery must be handled. User-facing ride and payment outcomes remain owned by their domain capabilities.
4. **Fast location state separate from durable ride state.** Geospatial/offer data needs different access and expiry behavior than the authoritative ride lifecycle. A cache outage must not silently erase the durable trip state.
5. **Replaceable external adapters.** Mapping, identity, payment, notification, and safety-response providers vary by market. The architecture avoids locking the product to one provider before market requirements are known.

## Architecture views

| View | Rendered diagram | Editable source |
|---|---|---|
| System context | [01-context-diagram.png](01-context-diagram.png) | [01-context-diagram.md](01-context-diagram.md) |
| Containers | [02-container-diagram.png](02-container-diagram.png) | [02-container-diagram.md](02-container-diagram.md) |
| Core ride components | [03-component-diagram.png](03-component-diagram.png) | [03-component-diagram.md](03-component-diagram.md) |
| Deployment pattern | [04-deployment-diagram.png](04-deployment-diagram.png) | [04-deployment-diagram.md](04-deployment-diagram.md) |

The Mermaid blocks in the source files are the editable source of truth. PNGs are compact visual companions that emphasize system boundaries and primary flows; the Mermaid views carry the more detailed interaction arrows. The PNGs were rendered locally because no Mermaid renderer is configured in the repository, and should be refreshed when the architecture changes.

## Assumptions and open decisions

| Item | Status | Architecture consequence |
|---|---|---|
| Minimal internal operations/support and market administration | Included by the product owner for this design; workflow details remain open. | Provide a role-scoped, audited internal access path; do not assume broad data access. |
| Worldwide ambition and prioritized countries | Confirmed ambition; exact cities, activation criteria, language, and jurisdictional obligations remain open (OD-01). | Use repeatable regional cells and per-market readiness gates; do not pin cells to named providers/regions. |
| Payment operations | In-app payment confirmed; methods, currencies, timing, recovery, refunds, settlement, and payouts remain open (OD-02). | Keep payment ownership isolated and represent pending/ambiguous outcomes explicitly. |
| Driver checks | Verification capability confirmed; market-specific checks remain open (OD-03). | Eligibility is market-aware and fail-closed when required policy is unavailable. |
| Safety response | Baseline capability confirmed; responder, coverage, escalation, and service promise remain open (OD-04). | Track actual action status; do not imply dispatch or guaranteed response. |
| Fare and cancellation rules | Remain open (OD-05). | Quote and ride capabilities own policy execution, but no specific policy is encoded here. |
| Internal roles and tools | Minimal operations/support capability included; detailed scope remains open (OD-06). | Keep access least-privilege and auditable; refine after workflows are approved. |
| Workload and quality targets | Remain open (OD-07). | No capacity sizing, replica counts, latency claims, availability promise, or RTO/RPO is asserted. |
| Commercial launch and operating owner | Remain open (OD-08). | This future-production design is not a launch-readiness assertion. |
| Residency, inter-region replication, and data sharing | Not resolved by Phase 0. | Cell placement and cross-cell data flows are illustrative only; obtain qualified market/privacy input before live operation. |
| Provider and hosting choices | Not selected. | All named technologies are candidates; deployment is cloud-neutral. |

## Phase boundary

Phase 1 documents system context, logical containers/components, a deployment pattern, responsibilities, trade-offs, assumptions, and open decisions. It does **not** define APIs, schemas, capacity calculations, cloud/provider choices, infrastructure configuration, deployment manifests, application code, or tests. Those belong to later phases and require the open product/NFR decisions where applicable.
