# Fundrise

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Fundrise is a Washington, DC based financial technology company that gives individual investors direct access to private-market alternative assets — private real estate, private credit, and venture capital — through a family of non-traded funds it sponsors and manages. Founded in 2012 and an early user of the SEC's Regulation A+ exemption, Fundrise packages institutional-grade private investments into low-minimum vehicles bought and held entirely online. Its developer-facing surface is Fundrise Connect, a REST API that lets banks, brokerages, investment apps and other financial platforms natively onboard their own end users (\"Clients\") into Fundrise offerings: create and update Clients, retrieve open Offerings and their historical daily NAV, fetch the offering documents and acknowledgments a Client must sign, place and cancel investments, request and cancel share liquidations, and read back holdings, transactions and tax forms. Access is gated — partners request credentials from the Fundrise Connect team — and the API is documented publicly with an OpenAPI 3.1 definition.

**Areas:** Company · Financial Services · Investing · Real Estate · Private Credit · Venture Capital · Alternative Assets · Wealth Management · Fintech · Embedded Investing

## Fundrise on APIs.io

Everything profiled for Fundrise is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/fundrise/](https://apis.io/providers/fundrise/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [Fundrise Acknowledgments API](https://apis.io/apis/fundrise/fundrise-acknowledgments-api/) — The Acknowledgments API provides acknowledgments needed to place Investments and Liquidations.
- [Fundrise Authentication API](https://apis.io/apis/fundrise/fundrise-authentication-api/) — ### Getting access To get started, please contact the support team via <a href="mailto:connect@fundrise.com" target="_blank">connect@fundrise.com</a>. ###…
- [Fundrise Clients API](https://apis.io/apis/fundrise/fundrise-clients-api/) — The Clients API provides operations for creating new Clients and updating fields for existing Clients. The Client represents an End-User of the platform.…
- [Fundrise Holdings API](https://apis.io/apis/fundrise/fundrise-holdings-api/) — The Holdings API contains up-to-date information about the status and performance of a Client's holdings in Fundrise. This endpoint may be used to fetch…
- [Fundrise Investments API](https://apis.io/apis/fundrise/fundrise-investments-api/) — The Investment APIs provide operations for placing and cancelling Investments in various Offerings on behalf of an authenticated Client.
- [Fundrise Liquidations API](https://apis.io/apis/fundrise/fundrise-liquidations-api/) — The Liquidations APIs provide operations for a Client to place a Liquidation request. A Liquidation request results in selling shares in exchange for dollars.
- [Fundrise Offerings API](https://apis.io/apis/fundrise/fundrise-offerings-api/) — The Offerings API contains information about all Offerings available via Fundrise Connect and documents associated with those offerings. Each Offering has a…
- [Fundrise Tax Forms API](https://apis.io/apis/fundrise/fundrise-tax-forms-api/) — The Tax Form API provides operations for fetching tax document information for a given tax year.
- [Fundrise Transactions API](https://apis.io/apis/fundrise/fundrise-transactions-api/) — The Transactions API provides operations for fetching Transactions (individual or bulk) on behalf of authenticated Clients.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Arazzo workflows (1)

Multi-step workflows that chain operations into a business task.

- [Fundrise Connect — onboard a Client and place an Investment](https://apis.io/arazzos/fundrise/fundrise-onboard-client-and-invest/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [fundrise-connect](https://apis.io/servers/fundrise/fundrise-connect/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Fundrise Agentic Access](https://apis.io/agentic-access/fundrise/fundrise-agentic-access/)

#### Collections (9)

Postman and Open Collection request collections.

- [Fundrise Connect (External API) Acknowledgments API](https://apis.io/collections/fundrise/open-fundrise-acknowledgments-api/)
- [Fundrise Connect (External API) Authentication API](https://apis.io/collections/fundrise/open-fundrise-authentication-api/)
- [Fundrise Connect (External API) Clients API](https://apis.io/collections/fundrise/open-fundrise-clients-api/)
- [Fundrise Connect (External API) Holdings API](https://apis.io/collections/fundrise/open-fundrise-holdings-api/)
- [Fundrise Connect (External API) Investments API](https://apis.io/collections/fundrise/open-fundrise-investments-api/)
- [Fundrise Connect (External API) Liquidations API](https://apis.io/collections/fundrise/open-fundrise-liquidations-api/)
- [Fundrise Connect (External API) Offerings API](https://apis.io/collections/fundrise/open-fundrise-offerings-api/)
- [Fundrise Connect (External API) Tax Forms API](https://apis.io/collections/fundrise/open-fundrise-tax-forms-api/)
- [Fundrise Connect (External API) Transactions API](https://apis.io/collections/fundrise/open-fundrise-transactions-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Fundrise Connect Overlay](https://apis.io/overlays/fundrise/fundrise-connect-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Fundrise Authentication](https://apis.io/security/fundrise/fundrise-authentication/)
- [Fundrise Domain Security](https://apis.io/security/fundrise/fundrise-domain-security/)
- [Fundrise Vulnerability Disclosure](https://apis.io/security/fundrise/fundrise-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Fundrise Scopes](https://apis.io/scopes/fundrise/fundrise-scopes/)

#### Rate limits (1)

Published request limits.

- [Fundrise Rate Limits](https://apis.io/rate-limits/fundrise/fundrise-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://fundrise.com/)
- [Documentation](https://connect.fundrise.com/)
- [Getting started](https://connect.fundrise.com/#tag/Authentication)
- [API reference](https://connect.fundrise.com/)
- [Sign up](https://fundrise.com/checkout-start)
- [Support](https://fundrise.com/help)
- [Blog](https://fundrise.com/education)
- [Security](https://fundrise.com/.well-known/security.txt)
- [Terms of service](https://fundrise.com/website-documents/Terms%20of%20Service)
- [Privacy policy](https://fundrise.com/privacy-policy.html)
- [GitHub](https://github.com/fundrise)

## API Evangelist rating

**Kin Score 38.6** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 52.4 |
| Contract governance | 4.5 |
| Operational transparency | 15.8 |
| Developer ergonomics | 51.8 |
| Access clarity | 17.1 |

**Agent readiness 36.4** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | documented (partial credit) |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 36.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/fundrise/](https://apis.io/providers/fundrise/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Fundrise: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/fundrise/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
