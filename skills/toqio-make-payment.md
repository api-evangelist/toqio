---
generated: '2026-07-21'
method: generated
name: Make a payment to a beneficiary
description: >-
  Create a beneficiary, quote FX if needed, submit the payment, and track its
  status.
api: openapi/toqio-transactions-openapi.yml
operations: [getBankPaymentSchemesUsingGET, createBeneficiaryUsingPOST, getQuoteForCurrencyPairUsingPUT, createTransactionUsingPOST, getTransactionUsingGET]
source: >-
  operationIds verified in openapi/toqio-beneficiaries-openapi.yml and
  openapi/toqio-transactions-openapi.yml.
---

# Make a payment to a beneficiary

Send a same-currency or FX payment from a client account.

## Auth
- OAuth 2.0 Client Credentials Bearer token (see `authentication/toqio-authentication.yml`); sandbox host `https://api.sandbox.toq.io/wallet/api` for testing (`sandbox/toqio-sandbox.yml`).

## Steps
1. **Check available payment schemes** — `getBankPaymentSchemesUsingGET` (`GET /wallet/api` bank payment schemes) for the account's currency/rails.
2. **Create the beneficiary** — `createBeneficiaryUsingPOST` with the payee's bank details; capture the beneficiary id. For transfers between platform accounts use `getInternalBeneficiaryUsingPOST`.
3. **Quote FX (only for cross-currency)** — `getQuoteForCurrencyPairUsingPUT` to lock a rate for the currency pair.
4. **Create the payment** — `createTransactionUsingPOST` with account, beneficiary, amount, and scheme. Four-eyes approval flows use `executeFourEyesPaymentUsingPOST` instead.
5. **Track it** — `getTransactionUsingGET` until the payment reaches a terminal status; `cancelTransferUsingDELETE` cancels a scheduled/pending transfer.

## Errors
- Insufficient balance and similar business errors surface field-level messages in the `data[]` envelope shown to end users; `requestId` traces the failure. See `errors/toqio-problem-types.yml`.

## Notes
- No idempotency keys: on timeout, list transactions before re-submitting (`conventions/toqio-conventions.yml`).
- Pagination on list endpoints is `pageNumber`/`pageSize`.
