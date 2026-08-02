---
generated: '2026-07-21'
method: generated
name: Onboard a client and open their first account
description: >-
  Create a client lead, track compliance, then open the client's first account
  on a partner product.
api: openapi/toqio-accounts-openapi.yml
operations: [createCompanyLeadViaAPIUsingPOST, getComplianceBlocksStatusesPerProvider, getPartnerProductsUsingGET_1, createAccountUsingPOST_1, getAccountsUsingGET]
source: >-
  operationIds verified in openapi/toqio-create-lead-service-openapi.yml,
  openapi/toqio-compliance-endpoints-openapi.yml, and
  openapi/toqio-accounts-openapi.yml.
---

# Onboard a client and open their first account

Onboard an SME client onto a Toqio-powered platform and open its first account.

## Auth
- OAuth 2.0 Client Credentials: `POST https://api.toq.io/iam/oauth/token` with `grant_type=client_credentials` and Basic auth; send the returned token as `Authorization: Bearer {access_token}` (expires in 3600s). See `authentication/toqio-authentication.yml`.
- Use the simulation environment (`https://api.sandbox.toq.io`) first. See `sandbox/toqio-sandbox.yml`.

## Steps
1. **Create the client lead** — `createCompanyLeadViaAPIUsingPOST` (`POST /customers/{customerId}/clients`) with the company details payload. The client enters compliance review.
2. **Check compliance** — `getComplianceBlocksStatusesPerProvider` (`GET` on the compliance endpoints) until the company's compliance blocks are approved. Upload KYB documents if requested (see `openapi/toqio-upload-kyb-documents-openapi.yml`).
3. **Pick a product** — `getPartnerProductsUsingGET_1` to list the partner products accounts can be created against.
4. **Create the account** — `createAccountUsingPOST_1` (wallet service) with the client and product ids.
5. **Verify** — `getAccountsUsingGET` to confirm the account is live and retrieve its details.

## Errors
- `401`/`403` mean a missing or insufficient token; `404` means an unknown `customerId`/`clientId`. Error payloads carry `code`, `description`, `requestId`, `status` — see `errors/toqio-problem-types.yml`.

## Notes
- No idempotency keys are supported — do not blindly retry POSTs; check for the created resource first (`conventions/toqio-conventions.yml`).
