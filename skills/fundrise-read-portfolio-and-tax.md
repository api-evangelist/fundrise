---
name: Read a Fundrise Client's portfolio, transactions and tax forms
description: Retrieve a Client's Fundrise holdings, transaction history and tax documents, and enrich them with offering and NAV data — the read-only reporting flow, safe for agent use.
api: openapi/fundrise-connect-openapi.yml
generated: '2026-08-04'
method: generated
source: openapi/fundrise-connect-openapi.yml
operations:
  - GetAccessToken
  - GetClient
  - GetHoldings
  - GetTransactions
  - GetTransaction
  - GetTaxForms
  - GetOfferings
  - GetHistoricalNav
---

# Read a Fundrise Client's portfolio, transactions and tax forms

The read-only half of Fundrise Connect. This is the part of the API genuinely suited to
agent use — nothing here moves money or changes state.

## Get a Client token

`GetAccessToken` — `POST /v1/oauth/token` — **PartnerBasicAuthentication**

Exchange the Client's stored non-expiring `refreshToken` for an `accessToken`. Every
operation below except `GetOfferings` and `GetHistoricalNav` uses that token as
**ClientBearerAuthentication**.

## Resolve the account

`GetClient` — `GET /v1/client` — **ClientBearerAuthentication**

Returns `clientId`, name, `primaryEmail`, `taxId`, `primaryAddress`, `dateOfBirth` and
`accounts[]`. Take `accountId` from `accounts[0]` — a Client currently has exactly one
account, and there is no separate account lookup operation.

**This response is PII, including a tax identifier.** Do not log it, do not echo it back
into a model context, and do not persist it beyond what you need. `accountId` is the only
value you actually need downstream.

## Holdings

`GetHoldings` — `GET /v1/account/{accountId}/holdings` — **ClientBearerAuthentication**

Per-offering positions: `offeringId`, `shares`, `costBasis`, `currentValue`,
`pendingValue`, `settledValue`, `unpaidDistributions`, `liquidable`.

- Values update **daily** with appreciation and accruing dividends. Present them as
  previous-close, not live.
- A `204` means the Client holds nothing — an empty portfolio, not an error.
- `pendingValue` vs `settledValue` matters: money in flight has not settled into the
  position yet. Do not sum them into a single "balance" without saying which is which.
- All monetary values are **strings**. Use decimal arithmetic, never a binary float.

## Enrich with offering data

Holdings carry only `offeringId`, so join against:

`GetOfferings` — `GET /v1/offerings` — **PartnerBasicAuthentication** — for
`offeringName`, `assetClass` (`REAL_ESTATE` / `PRIVATE_CREDIT` / `VENTURE`),
`currentPrice`, `currentDistributionRate`, `primaryObjective` and `status`.

`GetHistoricalNav` — `GET /v1/offering/{offeringId}/nav` — **PartnerBasicAuthentication** —
for a `historicalDailyNav` array of `effectiveDate` / `netAssetValuePerShare` pairs, if
you are charting performance.

Note the auth switch: these two are Partner-context, so cache them once per partner rather
than fetching them per Client.

## Transactions

`GetTransactions` — `GET /v1/account/{accountId}/transactions` — **ClientBearerAuthentication**

Each `TransactionResponse` carries `transactionId`, `amount`, `transactionType`
(`INVESTMENT` / `LIQUIDATION` / `DIVIDEND`), `transactionDate`, `description`, `status`
(`PENDING` / `COMPLETE` / `FAILED`), `debitCreditMemo` (`DEBIT` / `CREDIT`), `fees`,
`isCancellable`, and an `offerings[]` breakdown.

**There is no pagination and no date filter.** The endpoint returns a bare array of the
whole history. On a long-lived account this grows without bound, and there is no published
way to window it — budget for it, and filter client-side on `transactionDate`.

`GetTransaction` — `GET /v1/account/{accountId}/transaction/{transactionType}/{transactionId}`
fetches one transaction. Note the path requires the **type as well as the id**. A `404`
here means "not found *or* not owned by this account" — Fundrise conflates the two, so do
not report it to a user as "this transaction does not exist".

## Tax forms

`GetTaxForms` — `GET /v1/account/{accountId}/tax-forms` — **ClientBearerAuthentication**

Optional `taxYear` query parameter — the only filter parameter anywhere in this API.
Returns `TaxDocumentResponse` objects with `taxYear`, `documentId`, `documentTitle` and
`documentUrl`.

Treat `documentUrl` as sensitive. These are tax documents; hand the link to the person,
do not fetch the contents into a model context.

## Polling, because there are no events

Fundrise publishes no webhooks, no AsyncAPI and no event surface of any kind. Every state
change — a `PENDING` transaction settling, a dividend posting, a NAV update — is only
observable by polling. Combine that with per-Client, per-method rate limiting whose
numeric budget is **not published** and for which no `429` is declared in the spec, and
polling frequency becomes a judgement call. Poll conservatively; holdings only change
daily, so more than once a day buys you nothing.

## Guardrails

1. **Read-only, but not harmless.** Everything here is financial PII. Minimise what you
   pull into context and never persist tax identifiers.
2. **Do not give investment advice** off the back of this data. Reporting what a person
   holds is not the same as recommending what they should hold.
3. **Say when the data is stale.** Daily-updated values presented as live is a real
   misrepresentation in a financial context.
4. **A `403` is a permissions boundary,** not a bug — Fundrise grants endpoint access per
   partner. Do not retry it.
5. **Log the `Request-Id` header** (echoed as `referenceId` in error bodies) for support.
