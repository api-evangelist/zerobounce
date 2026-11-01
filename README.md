# ZeroBounce

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

ZeroBounce is an email validation and deliverability platform that verifies email addresses, detects spam traps and abuse accounts, scores leads, and helps senders reduce bounce rates and protect sender reputation. The platform provides real-time and batch email validation, credit balance checks, file-based validation jobs, email finder, domain search, activity data, list evaluation and AI scoring through a versioned REST API. The ZeroBounce v2 API uses a per-request api_key parameter with no OAuth or scopes, and is served from region-split hosts for US and EU data residency plus a separate bulk host for asynchronous file jobs. ZeroBounce also ships an official MCP server and first-party SDKs across nine languages.

**Areas:** Email Verification · Email Deliverability · Marketing · Lead Scoring · Anti-Spam · Data Quality · Email Finder · Deliverability Monitoring · DMARC · Email

## ZeroBounce on APIs.io

Everything profiled for ZeroBounce is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/zerobounce/](https://apis.io/providers/zerobounce/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [ZeroBounce Email Validation API](https://apis.io/apis/zerobounce/email-validation-api/) — Real-time and batch email validation API including credit balance, API usage, file submission and retrieval, email finder, domain search, activity data and…
- [ZeroBounce Email Validation API (US)](https://apis.io/apis/zerobounce/email-validation-api-us/) — US-region endpoint of the ZeroBounce v2 email validation API. Introduced October 2025 alongside the EU host so that, per ZeroBounce's own documentation,…
- [ZeroBounce Email Validation API (EU)](https://apis.io/apis/zerobounce/email-validation-api-eu/) — EU-region endpoint of the ZeroBounce v2 email validation API for customers requiring European data residency, where validations and interactions occur only…
- [ZeroBounce Bulk API](https://apis.io/apis/zerobounce/bulk-api/) — Asynchronous file-job surface for bulk email validation, bulk AI scoring, bulk email finder, bulk domain search, List Evaluator and Amazon S3 file…
- [ZeroBounce Validation API](https://apis.io/apis/zerobounce/zerobounce-validation-api/) — The ZeroBounce ChatGPT-plugin validation surface — 2 operations for single and batch email validation, served from members-api.zerobounce.net. The…
- [ZeroBounce MCP Server](https://apis.io/apis/zerobounce/mcp/) — Official ZeroBounce Model Context Protocol server, published by ZeroBounce under Apache-2.0 and distributed on npm as @zerobounce/mcp. Exposes 16 tools…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Zerobounce Webhooks](https://apis.io/asyncapis/zerobounce/zerobounce-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [ZeroBounce MCP Server](https://apis.io/servers/zerobounce/zerobounce-mcp-server/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Zerobounce Agentic Access](https://apis.io/agentic-access/zerobounce/zerobounce-agentic-access/)

#### Collections (2)

Postman and Open Collection request collections.

- [ZeroBounce Validate Email plugin](https://apis.io/collections/zerobounce/open-zerobounce/)
- [ZeroBounce Validate Email plugin Validation API](https://apis.io/collections/zerobounce/open-zerobounce-validation-api/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Zerobounce Validation Api Overlay](https://apis.io/overlays/zerobounce/zerobounce-validation-api-overlay/)
- [Zerobounce Validation Api Phrasing Overlay](https://apis.io/overlays/zerobounce/zerobounce-validation-api-phrasing-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Zerobounce Authentication](https://apis.io/security/zerobounce/zerobounce-authentication/)
- [Zerobounce Domain Security](https://apis.io/security/zerobounce/zerobounce-domain-security/)
- [Zerobounce Trust Center](https://apis.io/security/zerobounce/zerobounce-trust-center/)
- [Zerobounce Vulnerability Disclosure](https://apis.io/security/zerobounce/zerobounce-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Zerobounce Plans Pricing](https://apis.io/plans/zerobounce/zerobounce-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Zerobounce Rate Limits](https://apis.io/rate-limits/zerobounce/zerobounce-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.zerobounce.net)
- [Documentation](https://www.zerobounce.net/docs)
- [Getting started](https://www.zerobounce.net/docs/email-validation-api-quickstart/v2-validate-emails)
- [API reference](https://www.zerobounce.net/docs/api-dashboard/api-endpoints)
- [Pricing](https://www.zerobounce.net/pricing)
- [Sign up](https://www.zerobounce.net/members/createaccount)
- [Support](https://www.zerobounce.net/contact-us)
- [Blog](https://www.zerobounce.net/blog/feed)
- [Terms of service](https://www.zerobounce.net/terms-of-use)
- [Privacy policy](https://www.zerobounce.net/privacy-policy)
- [GitHub](https://github.com/zerobounce)
- [Status](https://status.zerobounce.net)

## API Evangelist rating

**Kin Score 67.1** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.7 |
| Contract quality | 44.9 |
| Contract governance | 18.2 |
| Operational transparency | 84.2 |
| Developer ergonomics | 74.4 |
| Access clarity | 92.1 |

**Agent readiness 32.8** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/zerobounce/](https://apis.io/providers/zerobounce/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for ZeroBounce: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/zerobounce/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
