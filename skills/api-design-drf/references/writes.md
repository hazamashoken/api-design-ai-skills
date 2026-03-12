# Writes, Idempotency, and Concurrency

## Idempotency

- `GET`, `PUT`, and `DELETE` should stay idempotent by contract.
- For unsafe `POST` writes that may be retried, define idempotency behavior explicitly.
- If the design calls for idempotency keys, implement and document them deliberately.

## Conditional Writes

- Use `ETag` and `If-Match` when concurrent edits can race.
- Re-check preconditions at the write boundary, not only before entering it.
- Distinguish missing preconditions from stale preconditions in error responses.

## Partial JSON or Structured Writes

- Be explicit about merge semantics.
- Do not assume clients know whether nested structures replace, merge, or patch in place.

## Async Job Pattern

For long-running writes:

- return `202 Accepted`
- create a job resource
- expose job status and terminal result
- keep retries safe and observable

## State Transitions

- Use action endpoints only when the operation is not a plain field mutation.
- State whether repeated transition calls are safe, idempotent, or conflict-prone.
