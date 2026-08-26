---
name: Browse the quip catalog
description: Search, look up and read product detail from the quip oral-care store without transacting.
api: mcp/quip-nyc-mcp.yml
surface: https://www.getquip.com/api/ucp/mcp
operations: [search_catalog, lookup_catalog, get_product]
generated: '2026-08-26'
method: generated
source: mcp/quip-nyc-tools-list.json
---

# Browse the quip catalog

Read-only. Nothing here creates a cart, a checkout, or a charge.

## Before the first call

Every tool on this endpoint requires a `meta.ucp-agent.profile` URI identifying your agent. The
server fetches it. A call without a resolvable profile returns JSON-RPC `-32001`
`UCP discovery failed` / `invalid_profile_url` with HTTP 422 — this gate fires **before** argument
validation, so a missing-profile error does not mean your arguments were wrong.

Pass buyer context on every call so prices and availability are correct:
`context.address_country` and `context.currency`.

## Steps

1. **Search** — call `search_catalog` with `catalog` containing a natural-language `query`, filter
   criteria, or both. At least one of query or filters is required.
2. **Page** — the response is deliberately truncated. Take `pagination.cursor` from the response and
   pass it back on the next `search_catalog` call. Only page when the user asks for more results.
3. **Resolve in bulk** — when you already hold several product or variant identifiers, call
   `lookup_catalog` once rather than looping `get_product`.
4. **Read detail** — call `get_product` for the full record of a single product. This is where you
   get the **product variant id**, which is the identifier a cart line item needs.

## Reading prices correctly

Every price on this surface is an **integer in ISO 4217 minor units** paired with a currency code.
`{"amount": 600, "currency": "USD"}` is **$6.00**, not $600. Divide by 100 for two-decimal
currencies before you say a number to the buyer. Zero-decimal currencies such as JPY are already
whole units. quip repeats this rule on all thirteen tool descriptions; getting it wrong is the
most likely way to mislead a buyer on this API.

## Alternatives without MCP

The store also documents an unauthenticated JSON surface, verified live:

- `GET /products/{handle}.json`
- `GET /collections/{handle}/products.json`
- `GET /collections/all`
- `GET /search?q={query}&type=product`
- `GET /sitemap.xml`

Product pages additionally carry a schema.org `Product` JSON-LD block with `sku`, `brand`,
`offers[]` (price, priceCurrency, availability) and `aggregateRating` — see
`json-ld/quip-nyc-product.jsonld`.

## Rate limits

Per IP, unquantified. Back off on `429`. quip publishes no numeric limit and returns no
`RateLimit-*` headers.
