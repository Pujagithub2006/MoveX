# MoveX User Flows

**Status:** Phase 0 working definition  
**Version:** 0.1  
**Last updated:** 2026-11-01

## How to read these flows

These flows describe product behavior and observable outcomes; they do not prescribe implementation. Cancellation, matching, fare, payment, verification, and emergency-response policies remain open where noted. Requirements are cross-referenced by ID from the [functional requirements](02-functional-requirements.md).

## Flow 1: Rider requests and completes an on-demand trip

**Primary actor:** Rider  
**Supporting actor:** Eligible driver  
**Preconditions:** Rider can access the service in an enabled market; any required account checks are satisfied.

1. Rider enters an origin and destination.
2. MoveX validates the locations and presents an estimate under the market's configured fare rules (FR-03).
3. Rider confirms an immediate point-to-point request; MoveX records it and begins matching (FR-04, FR-05).
4. Eligible drivers receive the offer. A driver accepts; the rider sees the driver's assignment and pickup progress (FR-05, FR-06).
5. Driver arrives and marks pickup; the trip begins and both parties see active-trip status/location (FR-06, FR-08).
6. Driver completes the trip; MoveX records completion and presents the payment outcome (FR-08, FR-09).

**Alternate and error paths**

- Invalid or unavailable origin/destination: explain the issue and allow correction; no request is created (FR-03).
- No eligible driver accepts before the configured matching window expires: show no-match status and next available action. Window and retry policy are open (FR-05).
- Rider or driver cancels: update both parties and apply only the cancellation policy once approved (FR-07; OD-05).
- Location becomes unavailable or stale during the trip: show a degraded/stale state rather than implying current location; thresholds and recovery are open (FR-06; NFR-03).
- Driver attempts an invalid trip-state transition: reject the transition and retain the prior valid state (FR-08).

## Flow 2: Driver becomes eligible and completes a trip

**Primary actor:** Driver  
**Preconditions:** Driver can access the service in a market where onboarding is available.

1. Driver submits required identity, vehicle, and eligibility information.
2. MoveX records the verification outcome and makes the driver's eligibility state visible to the driver (FR-02, FR-10).
3. If eligible, the driver can receive and accept immediate ride offers; if not, MoveX explains that the driver cannot accept rides and what action is available (FR-02, FR-05).
4. For an accepted offer, driver proceeds to pickup, marks arrival, begins the trip, and records completion (FR-06, FR-08).
5. Driver sees trip and payment-related status after completion (FR-09).

**Alternate and error paths**

- Required verification is missing, rejected, expired, or still pending: do not make the driver eligible; show a clear status and next action (FR-02).
- Driver declines or does not respond to an offer: keep the request available for other eligible drivers until the configured matching outcome (FR-05).
- Market-specific eligibility cannot be determined: do not represent the driver as eligible until the market's approved rules are applied (DEC-01, DEC-06, OD-03).

**Open policy:** Verification sources, evidence retention, review/appeal, and re-check cadence depend on market decisions (OD-03).

## Flow 3: In-app payment succeeds or needs recovery

**Primary actor:** Rider  
**Supporting actor:** Payment service/provider (not selected)

1. A completed trip is submitted for in-app payment processing (FR-09).
2. MoveX reports a successful payment and makes the result visible to the rider.
3. If the payment fails or its outcome is ambiguous, MoveX reports that payment needs attention and does not display a success state without confirmed success.
4. Rider follows the approved recovery path; the final payment state is recorded and visible.

**Alternate and error paths**

- Payment method is declined or unavailable: show failure and any allowed replacement/retry action; supported methods are open (OD-02).
- Provider response times out or is ambiguous: preserve a pending/unknown outcome until reconciled; do not blindly repeat a charge. Reconciliation and retry policy are open.
- Refund, adjustment, or driver payout is requested: process only under approved financial policies; these policies are not defined in this version (OD-02, OD-05).

## Flow 4: Rider shares trip or uses an emergency action

**Primary actor:** Rider  
**Supporting actors:** Trusted contact and designated safety-response operator (not yet confirmed)

1. During an active trip, rider chooses to share trip details.
2. MoveX exposes the approved trip information to the selected recipient and indicates whether sharing is active (FR-10).
3. If the rider needs help, they can invoke the in-trip emergency action (FR-10).
4. MoveX communicates the action's actual status and routes it according to the market's approved response procedure.

**Alternate and error paths**

- Trip sharing cannot be delivered: tell the rider it is not active and provide another available path if one is approved.
- Emergency action cannot be delivered or acknowledged: clearly show the delivery/acknowledgement state and follow the approved fallback/escalation procedure; do not imply that help has been dispatched if it has not.
- The market has no confirmed response owner or coverage: do not claim a staffed response or guaranteed emergency-service connection; this is a launch-blocking decision (OD-04, NFR-08).

## Traceability and unresolved rules

| Journey | Requirements | Decisions still needed |
|---|---|---|
| Rider request to completion | FR-03–FR-09 | Fare/quote rules, matching window, cancellation policy, location thresholds (OD-05, OD-07). |
| Driver eligibility and trip | FR-02, FR-05–FR-10 | Market checks and verification operations (DEC-06, OD-01, OD-03). |
| Payment | FR-09 | Methods, currencies, authorization/capture, recovery, refunds, settlement, and payouts (OD-02). |
| Safety | FR-10 | Verification standard, recipient, response owner, coverage, and escalation (OD-03, OD-04). |

For the complete decision register and product intent, see the [product overview](01-product-overview.md). These flows remain product-level definitions; system design and implementation belong to later phases.
