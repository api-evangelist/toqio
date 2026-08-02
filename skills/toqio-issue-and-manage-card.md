---
generated: '2026-07-21'
method: generated
name: Issue and manage a card
description: >-
  Issue a virtual or physical card against an account, activate it, and manage
  its lifecycle.
api: openapi/toqio-cards-openapi.yml
operations: [issueCardUsingPOST, activateCard, getCard, updateCard, suspendCard, cancelCard]
source: operationIds verified in openapi/toqio-cards-openapi.yml.
---

# Issue and manage a card

Issue a card for a client user against an account, then drive its lifecycle.

## Auth
- OAuth 2.0 Client Credentials Bearer token (see `authentication/toqio-authentication.yml`). Card endpoints are served from `https://api.toq.io/wallet` (sandbox: `https://api.sandbox.toq.io/wallet`).

## Steps
1. **Issue the card** — `issueCardUsingPOST` with client, account, user, and card type (virtual or physical). Capture the card id.
2. **Activate (physical cards)** — `activateCard` once the cardholder receives it.
3. **Inspect** — `getCard` for status and details; `getCardsByClient` lists all of a client's cards.
4. **Adjust** — `updateCard` for limits/settings; `updateCardAliasUsingPUT` renames it.
5. **Lifecycle** — `suspendCard` to block temporarily, `cancelCard` to terminate permanently.

## Events
- Card webhooks (via the Modulr BaaS integration) report authorisations (CARDAUTH), physical-card creation outcomes (CARDCREATION), and status changes (CARDSTATUSUPDATE) — see `asyncapi/toqio-webhooks.yml`.

## Errors
- `404` for unknown card/client ids; `401`/`403` for token problems. See `errors/toqio-problem-types.yml`.
