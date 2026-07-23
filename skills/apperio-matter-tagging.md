---
name: Organize matters with tags
description: List matter tag sets, create tags, and link or unlink them to matters in Apperio.
api: openapi/apperio-openapi-original.yml
operations:
  - 'GET /api/v1/filter/matter-tag-sets/'
  - 'GET /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/'
  - 'POST /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/'
  - 'POST /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/{matter-tag-id}/links'
  - 'DELETE /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/{matter-tag-id}/links/{link-id}'
---

# Organize matters with tags

Segment matters using Apperio matter tags for filtering and reporting.

## Auth
- `Authorization: Token <TOKEN VALUE>`.

## Steps
1. List tag sets with `GET /api/v1/filter/matter-tag-sets/` and inspect one via `GET /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/`.
2. Create tags in a set with `POST /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/`.
3. Link a tag to a matter with `POST /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/{matter-tag-id}/links`.
4. Unlink with `DELETE /api/v1/filter/matter-tag-sets/{matter-tag-set-id}/matter-tags/{matter-tag-id}/links/{link-id}` using the `link-id` returned when the link was created.
5. Rename a tag with `PUT .../matter-tags/{matter-tag-id}/`; remove it with `DELETE .../matter-tags/{matter-tag-id}/`.

## Rules
- Capture the `link-id` from the link-creation response so you can remove the association later.
- Page through tag/tag-set lists via `pagination.nextPage`; handle `429` with backoff.
