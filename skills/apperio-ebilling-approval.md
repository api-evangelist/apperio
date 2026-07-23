---
name: Review and approve an e-billing invoice
description: Retrieve an Apperio invoice, walk its approval workflow, and approve, reject, adjust, or query it.
api: openapi/apperio-openapi-original.yml
operations:
  - 'GET /api/v1/ebilling/invoices/{invoice-id}/'
  - 'GET /api/v1/ebilling/invoices/{invoice-id}/approval-workflow/'
  - 'POST /api/v1/ebilling/invoices/{invoice-id}/approval-state/{invoice-approval-state-id}/approve/'
  - 'POST /api/v1/ebilling/invoices/{invoice-id}/approval-state/{invoice-approval-state-id}/reject/'
  - 'POST /api/v1/ebilling/invoices/{invoice-id}/approval-state/{invoice-approval-state-id}/adjust/'
  - 'POST /api/v1/ebilling/invoices/{invoice-id}/approval-state/{invoice-approval-state-id}/query/'
---

# Review and approve an e-billing invoice

Drive an Apperio invoice through its approval workflow.

## Auth
- `Authorization: Token <TOKEN VALUE>`; e-billing approval actions require BUSINESS-side capability.

## Steps
1. Read the invoice with `GET /api/v1/ebilling/invoices/{invoice-id}/`.
2. Read `GET /api/v1/ebilling/invoices/{invoice-id}/approval-workflow/` to find the current `invoice-approval-state-id`.
3. Take one action against that state:
   - Approve: `POST .../approval-state/{invoice-approval-state-id}/approve/`
   - Reject: `POST .../approval-state/{invoice-approval-state-id}/reject/`
   - Adjust payable amount: `POST .../approval-state/{invoice-approval-state-id}/adjust/`
   - Query: `POST .../approval-state/{invoice-approval-state-id}/query/`
4. Optionally set custom attributes with `PUT /api/v1/ebilling/invoices/{invoice-id}/attributes/`.

## Rules
- Always fetch the approval workflow first to use the current approval-state id — acting on a stale state fails.
- There is no idempotency-key header; do not blind-retry a POST action after an ambiguous response — re-read the workflow to check state before retrying.
- On `403`, the token's user lacks the required capability. On `400`, correct the fields named in the error-message body.
