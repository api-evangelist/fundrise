---
name: Request and manage a Fundrise share liquidation
description: Check which of a Client's Fundrise holdings can be liquidated, collect the required acknowledgments, submit a share liquidation request, and cancel it if needed.
api: openapi/fundrise-connect-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/fundrise-connect-openapi.yml
operations:
  - GetAccessToken
  - GetHoldings
  - GetLiquidationAcknowledgments
  - CreateShareLiquidationRequest
  - CancelShareLiquidationRequest
  - GetTransactions
---

# Request and manage a Fundrise share liquidation

A liquidation sells a Client's shares back for dollars. This is the inverse of the
investment flow and is equally consequential — treat it as a money-moving operation that
requires human approval.

## Before you start

You need a Client-scoped access token. Exchange the Client's stored `refreshToken` at
`GetAccessToken` — `POST /v1/oauth/token` (**PartnerBasicAuthentication**) — and use the
returned `accessToken` as the bearer for every step below.

You also need the Client's `accountId`, available only from `GetClient` or from the
original `CreateClient` response.

## Step 1 — Check what is actually liquidable

`GetHoldings` — `GET /v1/account/{accountId}/holdings` — **ClientBearerAuthentication**

Each `HoldingResponse` carries `offeringId`, `shares`, `costBasis`, `currentValue`,
`pendingValue`, `settledValue`, `unpaidDistributions` and — the field that matters here —
**`liquidable`** (boolean).

**Do not offer a liquidation for a holding where `liquidable` is false.** These are
private-market funds; redemption is not guaranteed on demand. Check the flag first rather
than submitting and handling the rejection.

Note also that holdings values update **daily**, so `currentValue` is a
previous-close figure, not a live quote. Say so when you show it to a person.

A `204` means the Client has no holdings at all — there is nothing to liquidate. Handle it
as an empty portfolio, not an error.

## Step 2 — Fetch the liquidation acknowledgments

`GetLiquidationAcknowledgments` — `GET /v1/liquidation/acknowledgments` — **ClientBearerAuthentication**

Unlike the investment acknowledgments, this endpoint is **not** scoped to an offering —
it returns the acknowledgments for liquidation generally.

Present each acknowledgment to the Client and require an explicit checkbox acceptance,
the same standard Fundrise sets for investment acknowledgments.

## Step 3 — Submit the liquidation request

`CreateShareLiquidationRequest` — `POST /v1/account/{accountId}/liquidation` — **ClientBearerAuthentication**

The `ShareLiquidationRequest` body has two fields:

- `allAcknowledgmentsAccepted` (boolean) — **required**. Set this to true only when the
  Client has genuinely accepted every acknowledgment from step 2.
- `offerings[]` — an array of `ShareLiquidationOfferingRequest`, each with `offeringId`
  and `shares`. **One request can span multiple offerings**, which is a real difference
  from the investment flow.

`shares` is a string (`SharePrecision`), not a number — do not coerce it to a float.

A successful call returns `201`. Capture the `shareLiquidationRequestId` — it is the only
handle you have for cancelling.

**There is no idempotency key on this operation.** Unlike `CreateClient` and
`PlaceInvestment`, liquidation has no `partnerReferenceId`. A blind retry after a timeout
or a `500` may submit a **second** liquidation. On an ambiguous failure, poll
`GetTransactions` for a `LIQUIDATION` transaction before retrying.

## Step 4 — Cancel, if needed

`CancelShareLiquidationRequest` — `PUT /v1/account/{accountId}/liquidation/{shareLiquidationRequestId}/cancel` — **ClientBearerAuthentication**

Cancels a pending liquidation. A `404` means either the request id does not exist or the
account does not own it — Fundrise does not distinguish the two, so a 404 is not proof of
non-existence.

## Step 5 — Track settlement

`GetTransactions` — `GET /v1/account/{accountId}/transactions` — **ClientBearerAuthentication**

The liquidation appears as a `TransactionResponse` with `transactionType: LIQUIDATION`,
a `status` of `PENDING` → `COMPLETE` or `FAILED`, an `isCancellable` flag, and a per-
offering breakdown in `offerings[]`.

Fundrise publishes **no webhooks and no event surface**, so polling is the only option.
The collection has no pagination and no date filter, so on a long-lived account you will
be scanning the whole history each time.

## Guardrails

1. **Human-in-the-loop is required.** This sells a person's securities. Never execute it
   from an inferred intent.
2. **Never set `allAcknowledgmentsAccepted: true` on the Client's behalf.** That boolean
   is a legal attestation, not a formality.
3. **Check `liquidable` before promising anything.** Telling a person they can redeem a
   position that cannot be redeemed is worse than not offering the action.
4. **Be careful with retries** — there is no idempotency key on this operation.
5. **Log the `Request-Id` / `referenceId`** from every response for support escalation to
   `connect@fundrise.com`.
