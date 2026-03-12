# Queries, Pagination, and Versioning

## Pagination

- Use `CursorPagination` for mutable lists where stable traversal matters.
- Keep `PageNumberPagination` when the existing API already uses it and there is no explicit migration decision.
- Use page-number pagination only when offset-like UX is acceptable.
- Document default page size, max page size, and sort order.

## Filtering

- Keep filter names stable and domain-oriented.
- Validate important filters with serializers when correctness matters.
- Use `django-filter` when it improves consistency, not just because it exists.

## Ordering

- Expose only supported ordering fields.
- Guarantee deterministic ordering, especially for cursor pagination.
- State whether counts are exact, approximate, or omitted.

## Search

- Treat search as a documented contract, not a best-effort text box.
- Be explicit about searchable fields if clients depend on them.

## Versioning

- Decide versioning policy at the contract level first.
- Then map to DRF versioning classes only if needed.
- Avoid framework-driven version churn.
- Existing unversioned DRF routes are not automatically defects. Preserve them unless a migration is
  explicitly part of the task.
