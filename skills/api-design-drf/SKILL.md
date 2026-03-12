---
name: api-design-drf
description: Django REST Framework adapter for API execution details. Use after the API contract is chosen, or when the user explicitly needs DRF guidance on serializers, viewsets, permissions, drf-spectacular, exception handling, pagination, filtering, versioning, or API tests.
---

# API Design DRF Adapter

Use this skill to map a chosen API design into Django REST Framework.

This skill is not the source of the design itself. Start from
`../api-design-principles/SKILL.md` unless the user is already past design and is
asking purely about DRF execution.

## Use This Skill For

- mapping resources and actions into DRF views or viewsets
- choosing between function views, `APIView`, generic views, mixins, and viewsets
- choosing serializer boundaries for request and response payloads
- implementing auth and object-level authorization in DRF
- standardizing DRF error handling and validation responses
- mapping pagination, filtering, ordering, and versioning into DRF
- handling idempotent writes, conditional requests, and async job-style endpoints
- documenting the contract with drf-spectacular
- defining DRF API tests that protect the contract

## Adapter Rules

- Treat DRF as an implementation constraint, not the design source.
- Keep resource names, error codes, and versioning semantics independent from DRF internals.
- Respect established DRF conventions already relied on by the project unless the task explicitly
  includes a contract migration.
- Prefer explicit serializers over implicit request or response shapes.
- Prefer explicit permission boundaries over view-level role checks scattered through logic.
- Prefer one reusable exception-handling strategy for stable error envelopes.
- Keep framework notes subordinate to contract clarity.

## DRF Mapping Workflow

1. Confirm the canonical contract from the principles skill.
2. Choose the DRF surface:
   - `APIView` or function views for precise, non-CRUD operations
   - `GenericAPIView` or viewsets when the resource model is conventional
3. Define serializers for:
   - query params when validation matters
   - write payloads
   - read payloads
   - error details where the contract needs structure
4. Map auth and authorization:
   - authentication classes
   - permission classes
   - object-scoped checks
5. Map pagination, filtering, ordering, and versioning.
6. Publish the contract through drf-spectacular.
7. Add tests that verify behavior, not just status codes.

## Boundary Note

This adapter is HTTP-focused. If the request includes real-time socket design, keep the transport-level
semantics in `api-design-principles` and use this skill only for any accompanying DRF HTTP endpoints.

## Load Reference

- Read `references/drf.md` first for the adapter overview and navigation.
- Read `references/view-selection.md` when choosing between DRF view styles.
- Read `references/serializers.md` for request, response, query, and partial-update boundaries.
- Read `references/permissions.md` for auth, object permissions, and tenant scoping.
- Read `references/errors.md` for exception handling and stable error envelopes.
- Read `references/queries.md` for pagination, filtering, ordering, and versioning.
- Read `references/writes.md` for idempotency, optimistic locking, and async write semantics.
- Read `references/schema.md` for drf-spectacular patterns and schema discipline.
- Read `references/testing.md` for contract-focused DRF API tests.
