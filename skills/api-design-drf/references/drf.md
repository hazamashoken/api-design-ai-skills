# DRF Adapter Overview

## Reference Map

- `view-selection.md`: when to use function views, `APIView`, generics, mixins, or viewsets
- `serializers.md`: query serializer patterns, read/write splits, partial updates, nested payload rules
- `permissions.md`: authentication, permission classes, object checks, tenancy boundaries
- `errors.md`: exception handling, validation normalization, stable error contracts
- `queries.md`: pagination, filtering, ordering, search, versioning
- `writes.md`: idempotency keys, `ETag` and `If-Match`, async job patterns
- `schema.md`: drf-spectacular usage, reusable components, examples
- `testing.md`: contract tests, schema checks, permission coverage, concurrency coverage

## Goal

Preserve the API contract while implementing it in Django REST Framework.

Do not let DRF defaults silently define:

- route semantics
- error envelope
- auth boundaries
- pagination policy
- versioning behavior

## Route Mapping

- Use resource-oriented URLs even when the view implementation uses actions.
- Prefer `POST /resources/` over `/resources/create/`.
- Prefer `PATCH /resources/{id}/` over `/resources/{id}/update/`.
- Use action subresources only when the business operation is not a plain field mutation.

## Operating Principle

DRF should express the contract, not invent it. If a DRF convenience feature would
change the public API semantics, prefer the contract over the convenience.
