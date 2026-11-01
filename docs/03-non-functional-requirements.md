# MoveX Non-Functional Requirements

**Status:** Phase 0 working definition  
**Version:** 0.1  
**Last updated:** 2026-11-01

## Purpose

This document defines quality attributes and how they will be measured. Numeric production targets are intentionally **unresolved**: exact market coverage, expected load, operating model, and future live-launch intent/timing have not yet been confirmed. Do not treat example measurement methods as approved service-level objectives.

Each target should be agreed before architecture is finalized. Record the measurement window, percentile or error budget, workload, geography, and test method alongside the target.

## Requirements and target status

| ID | Quality attribute | Required measure | Target/status | Depends on |
|---|---|---|---|---|
| NFR-01 | Request responsiveness | P50/P95/P99 latency for quote, ride request, match updates, and payment-status reads, measured by market and under an agreed load. | **Open**; thresholds and workload not set. | OD-01, OD-07 |
| NFR-02 | Availability | Availability/error budget for booking, active-trip status, safety action, and payment journeys, reported separately where appropriate. | **Open**; service promise and measurement window not set. | OD-04, OD-07, OD-08 |
| NFR-03 | Location freshness | End-to-end age of rider/driver location shown to the other party; include degraded-network behavior and stale-data indication. | **Open**; freshness and degraded-mode thresholds not set. | OD-01, OD-07 |
| NFR-04 | Matching capacity | Requests, eligible drivers, and concurrent active trips handled per market; include match-time distribution and overload behavior. | **Open**; demand forecast and match-time target not set. | OD-01, OD-07 |
| NFR-05 | Payment correctness | Duplicate-charge rate, payment outcome reconciliation, and recovery handling for timeout/ambiguous provider responses. | **Open**; acceptable error budget, methods, and settlement model not set. | OD-02, OD-07 |
| NFR-06 | Resilience and recovery | Recovery time and recovery point for critical trip/payment records; behavior under dependency, network, and regional failures. | **Open**; RTO/RPO and failure scenarios not set. | OD-01, OD-07, OD-08 |
| NFR-07 | Security and privacy | Access-control review, transport/storage protection, auditability of sensitive actions, and data retention/deletion requirements. | Baseline controls required; precise policy and jurisdictional obligations **open**. | OD-01, OD-03, OD-08 |
| NFR-08 | Safety operation | Emergency-action delivery/acknowledgement and end-to-end response time, with coverage and escalation measurement. | **Open**; owner, coverage, escalation, and response target not set. | OD-01, OD-04 |
| NFR-09 | Market readiness | Per-market validation of currency, locale, service rules, identity checks, payment availability, and support coverage before enabling service. | Market-specific checklist required; DEC-06 provides priority countries, but exact cities, coverage, and detailed criteria remain **open**. | DEC-06, OD-01, OD-02, OD-03, OD-06 |
| NFR-10 | Observability | Ability to correlate a ride and its matching, location, safety, and payment events; dashboards/alerts for agreed service targets. | Required as a product-operability capability; metrics, retention, and alert thresholds **open**. | OD-02, OD-04, OD-07 |
| NFR-11 | Accessibility and localization | Supported languages, currencies, time zones, units, and accessibility criteria tested in each enabled market. | **Open**; locale list and conformance target not set. | OD-01 |

## Target-setting gate

Before any target is called production-ready:

1. Define market coverage, supported hours, and whether/when the future-production design will operate as a live service (OD-01, OD-08).
2. Record expected and peak workload by market, including active trips and location-update volume (OD-07).
3. Agree service-level indicators, measurement windows, percentiles/error budgets, and user-visible degradation behavior.
4. Assign owners and operational coverage for safety, payments, and service incidents (OD-02, OD-04, OD-06).
5. Validate security, privacy, and market-specific requirements with qualified owners before any live launch.

## Traceability

- Functional journeys and capabilities: [functional requirements](02-functional-requirements.md).
- Product and unresolved market/operating decisions: [product overview](01-product-overview.md#open-decisions).
- Included and excluded capabilities: [scope](04-scope.md).
- User-visible fallback paths: [user flows](05-user-flows.md).
