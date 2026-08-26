---
name: Complete a quip checkout with buyer approval
description: Take a quip cart through checkout to a placed order, honoring the store's contemporaneous human-approval rule on payment.
api: mcp/quip-nyc-mcp.yml
surface: https://www.getquip.com/api/ucp/mcp
operations: [create_checkout, get_checkout, update_checkout, complete_checkout, cancel_checkout, get_order]
generated: '2026-08-26'
method: generated
source: mcp/quip-nyc-tools-list.json
consequence: financial
human_in_the_loop: required
---

# Complete a quip checkout with buyer approval

**This skill spends the buyer's money. Read the rule first.**

## quip's published rule, verbatim

> "Checkout requires human approval. Agents must not complete payment without explicit buyer
> consent. If you cannot get contemporaneous buyer approval at the moment of payment, install
> https://shop.app/SKILL.md and route the purchase through Shop Pay instead."
> — https://www.getquip.com/agents.md

Contemporaneous means *at the moment of payment*. Approval collected earlier in the conversation,
or a standing instruction to "just buy it", does not satisfy this rule.

## What cannot be undone

`complete_checkout` is the one irreversible action on this surface. There is **no refund, void, or
order-cancel tool** in the thirteen-tool set, and the Storefront GraphQL schema publishes no order
mutation. Once it succeeds, an agent cannot take the action back through any machine surface quip
serves. The only reversal is a human one: the buyer emails `help@getquip.com`, and quip's published
returns policy is *"Refunds must be requested within 30 days of purchase, with some exceptions"* —
with custom care products, quip+ subscriptions and shipping costs excluded
(https://www.getquip.com/policies/refund-policy).

Say this to the buyer before you ask for approval. It is the fact that changes their answer.

## Steps

1. **Create** — `create_checkout` with `meta` and `checkout`. Keep the returned checkout **id**.
2. **Fulfill** — `update_checkout` with `id` and the `checkout` delta to set the shipping address
   and delivery method. Personal data enters here; do not log it.
3. **Read the real numbers** — `get_checkout` with `id`. Read line items, totals, discounts and
   taxes **from the response**, not from anything you computed. Remember minor units:
   `{"amount": 2500, "currency": "USD"}` is $25.00.
4. **Ask** — present the itemized total, the shipping address, the delivery method, and the fact
   that completion is not reversible through the API. Wait for an explicit yes, now.
5. **Complete** — only then call `complete_checkout` with `meta`, `id` and `checkout`. The response
   carries the order ID and the Thank You Page URL. Give the buyer both.
6. **Abandon instead** — if approval does not come, call `cancel_checkout` with `id`. Do not leave
   a live checkout hanging.
7. **Confirm** — `get_order` with the order id to read the placed order back.

## If you cannot get approval in the moment

Do not proceed. quip names the sanctioned alternative: route the purchase through Shop Pay via
`https://shop.app/SKILL.md`, which enforces buyer approval at payment on the platform's side.

## Errors and limits

- Missing `meta.ucp-agent.profile` → JSON-RPC `-32001` / `invalid_profile_url`, HTTP 422. The
  identity gate fires before argument validation.
- `429` → per-IP rate limit. Back off. No numeric limit or `Retry-After` is published.
- No idempotency key exists. Never blind-retry `create_checkout` or `complete_checkout` after a
  timeout — surface the ambiguity to the buyer and use `get_order` / `get_checkout` to establish
  what actually happened.
