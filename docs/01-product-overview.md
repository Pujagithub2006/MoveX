# MoveX Product Overview

**Status:** Phase 0 working definition  
**Version:** 0.1  
**Last updated:** 2026-11-01

## Purpose

MoveX is a ride-booking product for riders and drivers. The repository describes its engineering objective as a production-inspired backend that demonstrates distributed systems, microservices, event-driven communication, real-time location processing, distributed transactions, scalability, cloud-native deployment, and observability.

This document defines the product problem and boundaries before implementation. Phase 0 is documentation and decision-making only; it does not authorize code, infrastructure, provider selection, or deployment work.

## Product direction

MoveX will support **immediate, point-to-point rides** in a **multi-city/global launch from day one**, with **in-app payment processing** and **baseline rider/driver safety capabilities**. The precise launch markets, local operating rules, payment methods, and safety-response procedures are not yet defined.

Scheduled rides and pooled/shared rides are excluded from the initial product scope.

## Users and needs

| Persona | Need | Product outcome |
|---|---|---|
| Rider | Request an immediate ride from an origin to a destination, understand the expected fare, follow the trip, pay in the app, and access safety support. | A clear and dependable journey from request through completion and payment. |
| Driver | Become eligible to provide rides, receive and respond to suitable requests, conduct a trip, and receive payment-related status. | A clear workflow from onboarding and verification through trip completion. |

Operations, customer-support, and market-administration personas are not yet confirmed. See [open decisions](#open-decisions).

## Product outcomes and measures

The desired product outcome is to make an immediate ride request safely and reliably match with an eligible driver and complete as an in-app-paid trip. The project also aims to provide a credible system-design/backend engineering demonstration.

Success measures and targets are **open** until launch markets and operating assumptions are defined. Candidate measures include request-to-match conversion, time to match, completed-trip rate, payment success rate, safety-response handling, and rider/driver retention. No numeric target is approved in this version.

## Confirmed decisions

| ID | Decision | Source/status |
|---|---|---|
| DEC-01 | Initial service ambition is multi-city/global from day one. | Confirmed by product owner; exact coverage details unresolved. |
| DEC-02 | The initial ride model is on-demand, point-to-point only. | Confirmed by product owner. |
| DEC-03 | In-app payment processing is in scope. Payment methods, providers, settlement, timing, and regional availability remain open. | Confirmed by product owner; implementation details unresolved. |
| DEC-04 | Baseline safety capabilities are in scope: identity/driver verification, trip sharing, and an in-trip emergency action. Exact requirements and response operations remain open. | Confirmed by product owner. |
| DEC-05 | Scheduled rides and shared/pool rides are not part of the initial scope. | Confirmed by product owner's ride-model choice. |
| DEC-06 | India, the United States, Canada, the United Kingdom, Japan, China, South Korea, Germany, and Italy are priority markets within a broader worldwide-availability ambition from day one. | Confirmed by product owner; specific country/city coverage and market activation criteria remain open. |
| DEC-07 | MoveX is designed for future production; it is not currently defined as an immediate live commercial launch. | Confirmed by product owner; future launch decision and operating owner remain open (OD-08). |

## Working assumptions

These are planning assumptions, not approved business decisions:

- Riders and drivers use client applications connected to a backend service.
- A ride has one rider, one driver, one origin, and one destination.
- Driver eligibility and applicable service rules vary by market.
- Quotes, matching, trip state, location updates, and payment status need to be understandable to the relevant user.
- Numeric service, scale, and recovery targets will be selected after launch-market and usage assumptions are documented.

## Open decisions

| ID | Decision needed | Why it matters |
|---|---|---|
| OD-01 | Which additional countries/cities are covered by the broader worldwide-availability ambition, and what language, currency, regulatory, payment, safety, and operational readiness criteria gate availability in each? | Regulations, localization, support coverage, currencies, and availability targets. |
| OD-02 | Which payment methods and currencies are required in each launch market? When is payment authorized/captured, and what are refund and payout rules? | Payment flows, failure handling, financial records, and provider constraints. |
| OD-03 | What identity, driver, vehicle, and document checks are mandatory in each market? | Eligibility, safety, and regulatory readiness. |
| OD-04 | Who receives and responds to an emergency action, and what response-time/coverage promise can be made? | Safety UX, operations, and service-level commitments. |
| OD-05 | What fare model, quote-validity period, cancellation rules, and dispute process apply? | Rider/driver expectations and functional requirements. |
| OD-06 | Are operations/support and market-administration tools included in the product? | Operational readiness and role/access requirements. |
| OD-07 | What are the initial traffic, availability, latency, location freshness, recovery, retention, and accessibility targets? | Measurable non-functional requirements and system capacity planning. |
| OD-08 | Will the future-production design proceed to a live commercial launch, and who will own the service and its operations if it does? | Determines evidence, operational rigor, compliance sign-off, and launch gates. |

## Related documents

- [Functional requirements](02-functional-requirements.md)
- [Non-functional requirements](03-non-functional-requirements.md)
- [Scope](04-scope.md)
- [User flows](05-user-flows.md)
