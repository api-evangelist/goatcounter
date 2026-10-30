# GoatCounter

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

GoatCounter is open-source, privacy-friendly web analytics from Martin Tournoij, offered both as a free donation-supported hosted service at goatcounter.com and as a self-hostable Go binary backed by SQLite or PostgreSQL. It tracks pageviews without cookies or persistent personal identifiers. The unversioned JSON API, prefixed /api/v0 and served from each account's own subdomain, covers counting pageviews and events from a backend, reading dashboard statistics (totals, per-path hits, referrals, browser/system/location breakdowns), running asynchronous CSV and JSON exports with an incremental hit-ID cursor, and managing sites and users. Authentication is a per-site API key sent as an HTTP bearer token, and the provider publishes its own OpenAPI 2.0 contract at https://www.goatcounter.com/api.json.

**Areas:** Analytics · Page Views · Privacy · Statistics · Web Analytics · Open Source · Self-Hosted · Event · Data Export · Developer Tools

## GoatCounter on APIs.io

Everything profiled for GoatCounter is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/goatcounter/](https://apis.io/providers/goatcounter/)** — or go straight to one of the APIs or properties below.

### APIs (9)

- [GoatCounter Exports API](https://apis.io/apis/goatcounter/goatcounter-exports-api/) — The Exports API from GoatCounter — 3 operation(s) for exports.
- [GoatCounter Pageviews API](https://apis.io/apis/goatcounter/goatcounter-pageviews-api/) — The Pageviews API from GoatCounter — 1 operation(s) for pageviews.
- [GoatCounter Paths API](https://apis.io/apis/goatcounter/goatcounter-paths-api/) — The Paths API from GoatCounter — 1 operation(s) for paths.
- [GoatCounter Sites API](https://apis.io/apis/goatcounter/goatcounter-sites-api/) — The Sites API from GoatCounter — 2 operation(s) for sites.
- [GoatCounter Statistics API](https://apis.io/apis/goatcounter/goatcounter-statistics-api/) — The Statistics API from GoatCounter — 5 operation(s) for statistics.
- [GoatCounter Users API](https://apis.io/apis/goatcounter/goatcounter-users-api/) — The Users API from GoatCounter — 1 operation(s) for users.
- [GoatCounter Count API](https://apis.io/apis/goatcounter/goatcounter-count-api/) — The count API from GoatCounter — 1 operation(s) for count.
- [GoatCounter Export API](https://apis.io/apis/goatcounter/goatcounter-export-api/) — The export API from GoatCounter — 3 operation(s) for export.
- [GoatCounter Stats API](https://apis.io/apis/goatcounter/goatcounter-stats-api/) — The stats API from GoatCounter — 5 operation(s) for stats.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [goatcounter](https://apis.io/servers/goatcounter/goatcounter/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Goatcounter Agentic Access](https://apis.io/agentic-access/goatcounter/goatcounter-agentic-access/)

#### Collections (7)

Postman and Open Collection request collections.

- [GoatCounter API](https://apis.io/collections/goatcounter/open-goatcounter/)
- [GoatCounter Exports API](https://apis.io/collections/goatcounter/open-goatcounter-exports-api/)
- [GoatCounter Exports Pageviews API](https://apis.io/collections/goatcounter/open-goatcounter-pageviews-api/)
- [GoatCounter Exports Paths API](https://apis.io/collections/goatcounter/open-goatcounter-paths-api/)
- [GoatCounter Exports Sites API](https://apis.io/collections/goatcounter/open-goatcounter-sites-api/)
- [GoatCounter Exports Statistics API](https://apis.io/collections/goatcounter/open-goatcounter-statistics-api/)
- [GoatCounter Exports Users API](https://apis.io/collections/goatcounter/open-goatcounter-users-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Goatcounter Api Overlay](https://apis.io/overlays/goatcounter/goatcounter-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Goatcounter Authentication](https://apis.io/security/goatcounter/goatcounter-authentication/)
- [Goatcounter Domain Security](https://apis.io/security/goatcounter/goatcounter-domain-security/)
- [Goatcounter Vulnerability Disclosure](https://apis.io/security/goatcounter/goatcounter-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Goatcounter Plans Pricing](https://apis.io/plans/goatcounter/goatcounter-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Goatcounter Rate Limits](https://apis.io/rate-limits/goatcounter/goatcounter-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Goatcounter Finops](https://apis.io/finops/goatcounter/goatcounter-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.goatcounter.com/)
- [Documentation](https://www.goatcounter.com/help/api)
- [Getting started](https://www.goatcounter.com/help/start)
- [API reference](https://www.goatcounter.com/api2.html)
- [Pricing](https://www.goatcounter.com/#pricing)
- [Sign up](https://www.goatcounter.com/signup)
- [Support](https://www.goatcounter.com/contact)
- [Security](https://www.goatcounter.com/security.txt)
- [Terms of service](https://www.goatcounter.com/help/terms)
- [Privacy policy](https://www.goatcounter.com/help/privacy)
- [GitHub](https://github.com/arp242/goatcounter)
- [Status](https://www.goatcounter.com/status)

## API Evangelist rating

**Kin Score 51.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 45.0 |
| Contract governance | 4.5 |
| Operational transparency | 52.6 |
| Developer ergonomics | 61.3 |
| Access clarity | 60.5 |

**Agent readiness 28.4** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | — |
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
| Pagination Ratio | paginated |
| Operation Distinctiveness | — |

**Regulatory alignment 30.4** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/goatcounter/](https://apis.io/providers/goatcounter/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for GoatCounter: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/goatcounter/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
