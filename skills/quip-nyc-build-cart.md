---
name: Build and maintain a quip cart
description: Create a cart, add and adjust line items, and abandon it cleanly — without completing a purchase.
api: mcp/quip-nyc-mcp.yml
surface: https://www.getquip.com/api/ucp/mcp
operations: [create_cart, get_cart, update_cart, cancel_cart, get_product]
generated: '2026-08-26'
method: generated
source: mcp/quip-nyc-tools-list.json
---

# Build and maintain a quip cart

This skill writes. It does not take payment — a cart is not a purchase.

## Preconditions

- `meta.ucp-agent.profile` on every call (see the browse skill).
- Product **variant** ids from `get_product`, not product ids.
- Buyer context: `context.address_country`, `context.currency`.

## Steps

1. **Create** — `create_cart` with a `cart` object carrying the initial line items. The response
   returns a **server-assigned cart id**. Keep it; every later call needs it.
2. **Read back** — `get_cart` with `id` to confirm line items and totals landed as intended.
3. **Adjust** — `update_cart` with `id` and the changed `cart`. This one tool covers what the
   Storefront GraphQL API splits across eleven mutations (`cartLinesAdd`, `cartLinesUpdate`,
   `cartLinesRemove`, `cartDiscountCodesUpdate`, delivery-address mutations, and so on). Send the
   delta you want, then read back with `get_cart`.
4. **Abandon** — `cancel_cart` with `id` when the buyer changes their mind. Start a fresh
   `create_cart` afterwards; cancelling is not itself undoable.

## There is no idempotency key

quip's surface publishes none — no `Idempotency-Key` header, no idempotency field in any of the
thirteen input schemas. Safety comes from the **id discipline** instead: `create_cart` is the only
call that mints state, and every other call is addressed to an existing id. So:

- Call `create_cart` **once** and store the id before doing anything else.
- If a `create_cart` call times out with no response, do **not** blind-retry it — you may create a
  second cart. Prefer failing the turn and telling the user.
- `update_cart` and `cancel_cart` are addressed to an id, so retrying them is comparatively safe.

## Errors

Transport errors come back as JSON-RPC 2.0 error objects with `code`, `message` and `data`
(`data.code`, `data.content`, `data.continue_url`), alongside a non-2xx HTTP status — HTTP 422 was
observed for the agent-profile gate. This surface does **not** speak RFC 9457 `problem+json`.
