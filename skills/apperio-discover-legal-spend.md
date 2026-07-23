---
name: Discover and analyze legal spend
description: Authenticate to Apperio, discover matters and invoices, and pull aggregated legal-spend analytics.
api: openapi/apperio-openapi-original.yml
operations:
  - 'GET /api/v1/filter/engagements/'
  - 'GET /api/v1/filter/invoices/'
  - 'GET /api/v1/data/analytics'
  - 'GET /api/v1/matters/{matter-id}/header'
---

# Discover and analyze legal spend

Use the Apperio API to explore matters (engagements) and invoices, then retrieve aggregated spend analytics.

## Auth
- Send `Authorization: Token <TOKEN VALUE>` on every request. Create the token on the Apperio profile page; API access must be enabled by Customer Success.
- Use the sandbox host `https://sandbox.apperio.com/api/v1` for testing, `https://app.apperio.com/api/v1` for production.
- The token identifies the organisation (BUSINESS or LAW_FIRM); resources are restricted to what that org can access.

## Steps
1. Discover matters with `GET /api/v1/filter/engagements/`. Page with `page` / `page-size` (default 50) and sort with `ordering` (comma-separated fields; prefix `-` to reverse).
2. Optionally discover invoices with `GET /api/v1/filter/invoices/`.
3. For a specific matter, read `GET /api/v1/matters/{matter-id}/header`.
4. Pull aggregated analytics with `GET /api/v1/data/analytics`.

## Rules
- Follow `pagination.nextPage` (pass it as the `page` param) until it is null; do not parallelize requests on one token.
- Handle `HTTP 429 Too Many Requests` with backoff; the ceiling is 10,000 requests/hour per token.
- On `400`, read the field-keyed error messages in the JSON body and correct the offending parameters.
