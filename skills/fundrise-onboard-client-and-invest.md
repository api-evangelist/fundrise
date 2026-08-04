---
name: Onboard a Fundrise Client and place an Investment
description: Onboard an end user onto Fundrise Connect as a Client, present the required offering documents and acknowledgments, and place their first investment in a Fundrise offering.
api: openapi/fundrise-connect-openapi.yml
generated: '2026-08-04'
method: generated
source: https://connect.fundrise.com/ (Workflow Example tag)
operations:
  - CreateClient
  - GetAccessToken
  - GetOfferings
  - GetOfferingDocuments
  - GetInvestmentAcknowledgments
  - PlaceInvestment
---

# Onboard a Fundrise Client and place an Investment

This is the primary flow of the Fundrise Connect API. Fundrise publishes it as the
"Workflow Example" in its own documentation; the steps below follow that sequence and
every operation id is taken verbatim from the published OpenAPI 3.1.0.

## Before you start

- **You need Partner credentials.** HTTP Basic username and password issued by Fundrise
  support. Request access at <https://fundrise.com/connect-api/contact> or
  `connect@fundrise.com`. There is no self-service signup.
- **Base URL.** Sandbox is `https://sandbox.fundrise.com` — the only server the spec
  declares. Production is `https://api.fundrise.com`.
- **Two auth subjects.** `PartnerBasicAuthentication` for Partner-context calls,
  `ClientBearerAuthentication` for anything scoped to one end user.
- **Some endpoints may 403.** Fundrise grants endpoint permissions per partner. A 403 on
  a documented operation is a permissions question, not a defect. Do not retry it.

## Step 1 — Create the Client

`CreateClient` — `POST /v1/client` — **PartnerBasicAuthentication**

Send `partnerReferenceId`, `primaryEmail`, `firstName`, `lastName`, `taxId`,
`primaryAddress` and `dateOfBirth`.

- `partnerReferenceId` is **your own unique id for this person** and doubles as the
  idempotency key. It must **not contain PII** — use an opaque internal id, never an
  email address.
- On success you get back `clientId`, `refreshToken`, and `accounts[]`. A new Client has
  **exactly one** account; multiple accounts are on Fundrise's roadmap but not supported.
- **Store the `refreshToken` encrypted at rest.** It does not expire. Fundrise requires
  that it never be exposed to the Client or to any of their devices.
- **A `409` means the Client already exists** for that `partnerReferenceId`. Treat it as a
  successful no-op and resolve the existing Client — do not retry with a fresh key, that
  would create a duplicate investor.
- A `400` returns `validationErrors`, a map of field name to error messages. Malformed
  address and date-of-birth fields are the usual cause.

Keep the `accountId` from `accounts[0]`. There is no account lookup operation — this is
the only place you will get it, other than `GetClient`.

## Step 2 — Get a Client access token

`GetAccessToken` — `POST /v1/oauth/token` — **PartnerBasicAuthentication**

Exchange the stored `refreshToken` for an access token. The response
(`OAuth2AccessTokenResponse`) carries `accessToken`, `refreshToken`, `scope`, `tokenType`
and `expiresIn`.

Use `accessToken` as the bearer token for every remaining step. Refresh it when it
expires — the refresh token itself does not expire, so this exchange is repeatable.

## Step 3 — Find an offering

`GetOfferings` — `GET /v1/offerings` — **PartnerBasicAuthentication**

Returns every offering available through Connect. Filter to `status: OPEN` — `CLOSED`
offerings cannot be invested in. Each `OfferingResponse` carries `offeringId`,
`offeringName`, `assetClass` (`REAL_ESTATE`, `PRIVATE_CREDIT` or `VENTURE`),
`currentPrice`, `currentDistributionRate`, `primaryObjective`, and
`minimumInvestmentAmount` / `maximumInvestmentAmount`.

The minimum and maximum are enforced **per transaction**. Validate the amount against
them before step 6 rather than letting Fundrise reject it.

Optionally call `GetHistoricalNav` — `GET /v1/offering/{offeringId}/nav` — for the daily
NAV history if you are showing performance.

## Step 4 — Fetch the offering documents

`GetOfferingDocuments` — `GET /v1/offering/{offeringId}/documents` — **PartnerBasicAuthentication**

Returns `DocumentResponse` objects with `documentId`, `documentName`, `documentType`
(`AGREEMENT`, `DISCLOSURE`, `PROSPECTUS`, `OFFERING_CIRCULAR`, …) and `documentUrl`.

**You must actually present these to the end user.** Collect the `documentId` of each —
they go into the investment request as `acknowledgedDocumentIds`.

## Step 5 — Fetch and collect the acknowledgments

`GetInvestmentAcknowledgments` — `GET /v1/offering/{offeringId}/acknowledgments` — **ClientBearerAuthentication**

Returns the acknowledgment text the Client must digitally accept. Fundrise is explicit
about what "digitally signed" means here: the Partner platform must display a checkbox
per acknowledgment and require the user to check it to proceed.

## Step 6 — Place the investment

`PlaceInvestment` — `POST /v1/account/{accountId}/investment` — **ClientBearerAuthentication**

Body requires `partnerReferenceId`, `amount`, `offeringId` and `acknowledgedDocumentIds`.

- `partnerReferenceId` here is a **second, distinct** idempotency key — this one scopes
  the investment, not the Client. Generate one per intended investment and reuse it on
  retry.
- `amount` is a **string** in US dollars. All monetary values in this API are strings to
  avoid float rounding; do not convert to a binary float.
- On a `500`, retrying with the **same** `partnerReferenceId` is safe.

## Guardrails

1. **Never auto-accept disclosures.** `acknowledgedDocumentIds` is a required field
   precisely because a person is supposed to have read and accepted those documents.
   Populating it without a real human acceptance defeats a securities-law control. If you
   are an agent, stop here and hand back to the user.
2. **Require human approval before step 6.** This operation moves real money into a
   private-market security. It should be human-in-the-loop, not conditional.
3. **Use sandbox first.** `https://sandbox.fundrise.com` is test mode and does not affect
   live data. Note that Fundrise publishes no test values, no fixtures and no test
   clocks — you will have to discover offering ids at runtime from `GetOfferings`.
4. **Trace everything.** Every response carries a `Request-Id` header, echoed as
   `referenceId` in error bodies. Log it. It is the only required field on the error
   envelope and the fastest route to support.
5. **Never log or return the `refreshToken` or the Partner password.**

## Verify

- `GetHoldings` — `GET /v1/account/{accountId}/holdings` — the new position. Values update
  **daily**, not in real time, so do not expect an immediate change. A `204` means the
  Client holds nothing yet.
- `GetTransactions` — `GET /v1/account/{accountId}/transactions` — the investment appears
  as a transaction with `transactionType: INVESTMENT` and `status: PENDING`, moving to
  `COMPLETE` or `FAILED`. There is no webhook or event surface, so you must poll.
