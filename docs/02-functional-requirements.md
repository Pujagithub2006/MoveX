# MoveX Functional Requirements

**Status:** Phase 0 working definition  
**Version:** 0.1  
**Last updated:** 2026-11-01

## Purpose and conventions

This document describes observable product behavior, not architecture or implementation. Requirements marked **Proposed** are a baseline for product-owner review; they are not confirmed business policies. Items depending on unresolved decisions are identified rather than silently specified.

Priorities use **P0** (required for the initial product outcome) and **P1** (important, but can follow the core flow). Priority labels are proposed pending review.

## Requirements

| ID | Priority / status | Requirement | Acceptance evidence / unresolved dependency |
|---|---|---|---|
| FR-01 | P0 / Proposed | A rider can create an account, sign in, and manage the account needed to request a ride. | Demonstrate successful and rejected authentication paths. Identity, age, and account-recovery policies are open. |
| FR-02 | P0 / Proposed | A driver can submit the information required to become eligible to receive ride requests. The system must prevent an ineligible driver from accepting requests. | Demonstrate eligible and ineligible states. Required checks/documents vary by market (DEC-01, DEC-06, OD-03). |
| FR-03 | P0 / Proposed | A rider can enter an origin and destination and receive a fare estimate before confirming an immediate ride request. | Demonstrate valid and invalid locations and a returned estimate. Fare model and quote validity are open (OD-05). |
| FR-04 | P0 / Confirmed model; behavior proposed | A rider can request an immediate, single-rider, point-to-point trip. Scheduled and pooled/shared requests are not supported in the initial scope (DEC-02, DEC-05). | Demonstrate request creation and rejection of unsupported request types. |
| FR-05 | P0 / Proposed | The system can offer an open ride request to eligible drivers and record acceptance, rejection, expiry, or lack of a match. | Demonstrate one accepted and one no-match/expired request. Matching rules, radius, and timeout targets are open. |
| FR-06 | P0 / Proposed | Rider and driver can see the trip's current status and relevant location updates while a trip is active. | Demonstrate the state transitions in [Flow 1](05-user-flows.md#flow-1-rider-requests-and-completes-an-on-demand-trip). Location permission, freshness, and outage behavior are open. |
| FR-07 | P0 / Proposed | Rider and driver can cancel a trip/request where permitted, and the system records the resulting state. | Demonstrate cancellation before pickup and during an active trip. Eligibility, fees, and dispute rules are open (OD-05). |
| FR-08 | P0 / Proposed | A driver can mark arrival, begin the trip, and complete it; the rider can see the corresponding trip state. | Demonstrate the ordered state transitions and reject invalid transitions. |
| FR-09 | P0 / Confirmed capability; policy proposed | The rider can pay in the app for a completed ride and see whether the payment succeeded or needs attention (DEC-03). | Demonstrate success and failure paths. Methods, currency, authorization/capture timing, refunds, and driver payouts are open (OD-02). |
| FR-10 | P0 / Confirmed capability; details proposed | The product provides identity/driver verification, trip sharing, and an in-trip emergency action as baseline safety capabilities (DEC-04). | Demonstrate the user-facing availability and recorded outcome of each action. Verification standards and emergency response ownership/coverage are open (OD-03, OD-04). |
| FR-11 | P1 / Proposed | The rider and driver receive status notifications for material request, trip, and payment events. | Demonstrate notifications for acceptance, cancellation, completion, and payment failure. Channels and delivery guarantees are open. |
| FR-12 | P1 / Open | Operations/support users can investigate trip, payment, and safety issues if those personas and tools are included. | Product-owner decision required (OD-06); no support console is assumed in the initial scope. |

## Core product rules

The following are proposed rules to validate, not finalized policies:

- A trip can move through request, matching, accepted, pickup, in-progress, completed, or cancelled states; exact state names and allowed transitions require review.
- Only an eligible driver can accept a request.
- A completed trip can have a payment outcome other than success; payment failures must remain visible and recoverable rather than being represented as a successful charge.
- A safety action must not be presented as a guaranteed emergency-service response until coverage, ownership, and response times are confirmed.
- Market-specific restrictions must be configurable or explicitly handled before enabling a market; exact regulatory requirements are not defined here.

## Traceability

| Product decision / journey | Requirements |
|---|---|
| Immediate point-to-point ride (DEC-02) | FR-03 through FR-08 |
| Multi-city/global ambition (DEC-01) | FR-02, FR-03, FR-05, FR-10; market activation rules remain open |
| Product-owner-prioritized markets (DEC-06) | FR-02, FR-03; exact city and locale coverage remains open (OD-01) |
| In-app payments (DEC-03) | FR-09 |
| Baseline safety (DEC-04) | FR-02, FR-10 |
| Rider request-to-completion | [Flow 1](05-user-flows.md#flow-1-rider-requests-and-completes-an-on-demand-trip) |
| Driver eligibility and trip | [Flow 2](05-user-flows.md#flow-2-driver-becomes-eligible-and-completes-a-trip) |
| Payment outcome | [Flow 3](05-user-flows.md#flow-3-in-app-payment-succeeds-or-needs-recovery) |
| Safety action | [Flow 4](05-user-flows.md#flow-4-rider-shares-trip-or-uses-an-emergency-action) |

See the [non-functional requirements](03-non-functional-requirements.md) for quality attributes and unresolved targets, and [scope](04-scope.md) for phase boundaries.
