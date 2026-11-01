# MoveX Product Scope

**Status:** Phase 0 working definition  
**Version:** 0.1  
**Last updated:** 2026-11-01

## Scope objective

Establish the product boundary for an on-demand, point-to-point ride-booking service with in-app payment processing and baseline safety capabilities, intended for a multi-city/global launch. This scope describes product outcomes, not architecture, implementation, infrastructure, or vendor choices.

## In scope for the initial product definition

| Capability | Scope | Status / traceability |
|---|---|---|
| Rider and driver journeys | Define the request, match, trip, and completion experiences. | Baseline proposed; see FR-01–FR-08 and [user flows](05-user-flows.md). |
| Multi-city/global service ambition | Define a product intended for broader worldwide availability from day one. | Confirmed ambition (DEC-01, DEC-06); India, United States, Canada, United Kingdom, Japan, China, South Korea, Germany, and Italy are owner-prioritized; exact additional country/city, locale, and market-readiness coverage remains open (OD-01). |
| Immediate point-to-point rides | One rider requests one immediate trip from origin to destination. | Confirmed model (DEC-02); see DEC-05 and FR-04. |
| Driver eligibility | Capture the concept of driver verification and market eligibility. | Baseline safety capability confirmed (DEC-04); exact checks open (OD-03). |
| Matching and active-trip status | Find an eligible driver, communicate trip states, and handle no-match/cancellation paths. | Proposed baseline; matching policies remain open (FR-05–FR-08). |
| In-app payment | Process a ride payment in the app and expose success/failure status. | Confirmed capability (DEC-03); payment policy and methods open (OD-02). |
| Baseline safety | Identity/driver verification, trip sharing, and an in-trip emergency action. | Confirmed capabilities (DEC-04); regional standards and response operations open (OD-03, OD-04). |
| Quality targets | Define measurable performance, availability, location, resilience, security, and operability requirements. | Target-setting required; numeric targets remain open (NFR-01–NFR-11). |

## Explicitly out of scope for the initial product

| Capability | Boundary |
|---|---|
| Scheduled rides | Not supported in the initial ride model (DEC-05). |
| Shared, pooled, or multi-rider rides | Not supported in the initial ride model (DEC-05). |
| Phase 0 implementation | No code, infrastructure, integrations, provider selection, or deployment is part of this phase. Phase 0 produces product-definition documents only. |

No other feature is treated as explicitly excluded until the product owner decides it.

## Unresolved boundary decisions

- Whether customer support, operational tooling, and market administration are part of the initial product (OD-06).
- Whether fare rules, cancellation fees, refunds, driver payouts, promotions, ratings, or disputes are included and under what policies (OD-02, OD-05, OD-06).
- Exact country/city, language, and currency coverage for broader worldwide availability, including market-readiness criteria (OD-01).
- Whether and when the future-production design will proceed to live commercial operation, and who will own that service (OD-08).
- Numeric launch and service-quality thresholds (OD-07 and [NFRs](03-non-functional-requirements.md)).

## Phase boundary and exit criteria

Phase 0 is complete when:

- The five product-definition artifacts are consistent and reviewed.
- Product owner decisions are either confirmed or recorded with an owner and due point before they block implementation.
- Functional requirements have observable acceptance evidence and trace to journeys.
- Non-functional targets are measurable, or each unresolved target is explicitly documented with the missing decision.
- Included and excluded scope is agreed; unresolved market, payment, safety, and operations obligations are not represented as settled.

## Related documents

- [Product overview and decisions](01-product-overview.md)
- [Functional requirements](02-functional-requirements.md)
- [Non-functional requirements](03-non-functional-requirements.md)
- [User flows](05-user-flows.md)
