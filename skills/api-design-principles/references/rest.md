# REST API Design

## Use REST When

- The domain maps cleanly to resources with stable identifiers.
- Clients mostly perform CRUD, list, search, and explicit state transitions.
- HTTP caching, conditional requests, and standard status codes are useful.
- The API should be easy to consume from many clients without custom tooling.

## Model Resources First

- Name resources with nouns, not verbs.
- Give each resource a stable identifier and a canonical URL.
- Model collections separately from individual resources.
- Expose domain state transitions explicitly when they are not simple field updates.
- Do not mirror database tables blindly. Model resources around client and business needs, not storage layout.

Prefer:

- `GET /orders`
- `GET /orders/{orderId}`
- `POST /orders`
- `POST /orders/{orderId}/cancel`

Avoid:

- `POST /getOrders`
- `POST /orders/updateStatus`

## Choose Methods Deliberately

- Use `GET` for safe reads.
- Use `POST` for creation or non-idempotent actions.
- Use `PUT` for full replacement when the client sends the full new representation.
- Use `PATCH` for partial updates.
- Use `DELETE` for deletion or archival when deletion semantics are acceptable.

Be explicit about idempotency:

- `GET`, `PUT`, and `DELETE` should be idempotent by contract.
- `POST` is not idempotent unless you add an idempotency key.

## Shape Payloads Clearly

- Keep request and response schemas stable and explicit.
- Use consistent field names across resources.
- Prefer ISO 8601 timestamps with timezone information.
- Represent money, units, enums, and references consistently across the API.
- Include server-generated fields only where they matter.

For write operations, separate:

- Client-supplied input
- Server-generated metadata
- Expandable linked resources

## Use Status Codes Precisely

Common defaults:

- `200 OK` for successful reads and updates with a response body
- `201 Created` for creation with a `Location` header when possible
- `202 Accepted` for async processing
- `204 No Content` for successful operations without a response body
- `400 Bad Request` for malformed input
- `401 Unauthorized` for missing or invalid authentication
- `403 Forbidden` for authenticated but disallowed access
- `404 Not Found` for missing resources
- `409 Conflict` for version or state conflicts
- `422 Unprocessable Entity` for semantically invalid input
- `429 Too Many Requests` for throttling
- `500 Internal Server Error` for unexpected server-side failures

Do not overload `200` for every failure case.

## Design Errors As Contracts

Keep the base error object small and predictable:

- `code`: a stable application error code in `UPPER_SNAKE_CASE`
- `status`: the HTTP status code repeated in the payload for clients and logs that only inspect the body
- `message`: a human-readable explanation safe to show to operators and, when appropriate, end users

Example shape:

```json
{
  "error": {
    "code": "ORDER_STATE_CONFLICT",
    "status": 409,
    "message": "Only pending orders can be cancelled.",
    "request_id": "req_01HTQX6RK4F3A9Z9M9K6Q2P7NV",
    "details": {
      "current_state": "shipped"
    }
  }
}
```

Use optional extensions deliberately:

- `request_id`: helps support and debugging by giving every failed request a unique handle that can be searched in logs, traces, and upstream service telemetry.
- `request_id` creation: generate it at the edge of the request path, usually in API middleware, gateway infrastructure, or a load balancer. Reuse an inbound correlation header when you trust it, otherwise create a new ID such as a UUIDv4 or ULID and propagate it through logs and downstream calls.
- `details`: include structured, machine-readable context only when the client can act on it, such as validation failures, conflicting state, or quota metadata.
- `details` guidance: keep it stable, avoid dumping raw stack traces or internal exceptions, and do not expose secrets, SQL, or internal service topology.

For validation-heavy endpoints, a typical `details` shape is:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "status": 422,
    "message": "One or more fields are invalid.",
    "details": {
      "fields": [
        {
          "field": "email",
          "code": "INVALID_FORMAT",
          "message": "Email must be a valid address."
        }
      ]
    }
  }
}
```

If an existing API already uses a simpler framework-default error shape and clients depend on it,
preserve that shape unless the task explicitly includes a migration to a custom envelope. A custom
error object is best treated as a recommended pattern for new or intentionally redesigned public APIs,
not as a universal retrofit requirement.

## Support Lists Well

- Use filtering, sorting, and pagination consistently across list endpoints.
- Prefer cursor pagination for frequently changing datasets when the project is prepared to support it.
- Preserve established page-number pagination when it is already part of the contract and there is
  no explicit decision to migrate.
- Use limit and cursor parameters with explicit defaults and max bounds.
- Define sort keys and ordering deterministically.

Examples:

- Page-based pagination when datasets are stable enough: `GET /users?page=1&limit=10`
- Cursor pagination for frequently changing feeds: `GET /events?cursor=evt_123&limit=50`
- Filtering and sorting via query params: `GET /orders?status=pending&sort=-created_at`

State clearly:

- Default page size
- Maximum page size
- Sort stability guarantees
- Whether total counts are exact, approximate, or omitted

## Handle Concurrency And Retries

- Use idempotency keys for unsafe retries on create or command endpoints.
- Use ETags or version fields for optimistic concurrency when updates can conflict.
- Document retryable versus non-retryable failures.
- Avoid silent last-write-wins unless it is intentional and acceptable.

## Design Auth And Authorization

- Keep authentication transport-specific but authorization domain-specific.
- Use secure token-based or session-based authentication appropriate to the system.
- Define who can read, create, update, or transition each resource.
- Enforce authorization on the server for every request. Do not rely on client-side checks for protection.
- Avoid leaking existence through inconsistent `403` and `404` behavior.
- Document tenancy boundaries explicitly.
- Confirm that list and detail queries cannot leak data across tenants, organizations, or equivalent
  isolation boundaries.

## Plan Evolution

- Prefer additive changes: new fields, new endpoints, new enum values with backward-compatible handling.
- Treat field removal, meaning changes, and required new inputs as breaking.
- For new externally consumed REST APIs, explicit versioning may be a good default; state the
  mechanism clearly, such as path, header, or media type versioning.
- For existing unversioned APIs, lack of versioning is not automatically a defect. Treat version
  introduction as a compatibility-sensitive change that needs an explicit migration plan.
- Path versioning such as `/api/v1/users` is often the simplest choice when clients, gateways,
  or docs benefit from obvious version boundaries.
- Publish deprecation timelines for breaking changes.

## Review Checklist

- Do routes reflect the domain model rather than controller methods?
- Are method semantics and idempotency correct?
- Are list endpoints consistent in filtering and pagination?
- Are status codes and errors predictable?
- Is the auth model clear?
- Can the API evolve without immediate breaking changes?
