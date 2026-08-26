# Quip NYC

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

quip NYC Inc. is a Brooklyn, New York oral care company founded in 2015 by Simon Enever and Bill
May, selling dentist-designed electric toothbrushes, water flossers, refillable floss, toothpaste,
mouthwash and whitening products direct to consumers on a recurring refill subscription.

**quip publishes no developer portal, API reference, OpenAPI specification or SDK of its own.** Its
entire machine-readable surface is its storefront, and that surface is real and callable today:

- **UCP Commerce MCP API** — `https://www.getquip.com/api/ucp/mcp`. An anonymous Model Context
  Protocol endpoint implementing the Universal Commerce Protocol shopping service, with **13 tools**
  (catalog search/lookup, product detail, cart lifecycle, checkout lifecycle, order retrieval), each
  carrying a full JSON Schema 2020-12 input contract. `tools/list` and `initialize` are anonymous;
  every `tools/call` requires a resolvable UCP agent profile URI.
- **Storefront GraphQL API** — `https://www.getquip.com/api/2026-07/graphql.json`. Open anonymous
  introspection: 415 types, 34 query root fields, 41 mutations.
- **Storefront JSON endpoints** — documented, unauthenticated product and collection JSON.
- **Agent documentation** — `/agents.md` (canonical) mirrored at `/llms.txt`, a UCP merchant profile
  at `/.well-known/ucp`, and OIDC / RFC 8414 / RFC 9728 metadata under `/.well-known/`.

quip's published agent policy requires **contemporaneous human approval** before any agent completes
a payment, and there is **no API-level reversal of a completed checkout** — no refund, void or
order-cancel tool exists. Reversal is a human channel only, within the 30-day window on quip's
returns policy.

### Namesake warning

Searching for "Quip API" returns `quip.com/api/reference`, the **Quip Automation API**. That is
Salesforce's Quip document-collaboration product — a **different company**. Nothing from `quip.com`
belongs in this repository.

- https://www.getquip.com/
- https://www.getquip.com/agents.md
- https://github.com/getquip
- https://www.hiive.com/securities/quip-nyc-stock (harvest source)
