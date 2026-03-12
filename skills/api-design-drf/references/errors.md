# Errors and Exception Handling

## Goal

Expose one stable error contract even though DRF raises different internal exceptions.

## Default DRF Behavior

DRF gives useful defaults, but the raw shapes differ between:

- serializer validation errors
- authentication failures
- permission denials
- not found errors
- throttling
- custom business exceptions

That is often not a good public contract by itself.

It can still be the correct contract to preserve when an existing API already exposes DRF default
payloads and clients depend on them.

## Recommended Pattern

- use a shared exception handler
- normalize common DRF exceptions into one envelope
- keep stable application error codes separate from raw exception class names

Use this pattern for new or intentionally redesigned APIs. For established DRF APIs, preserving
`detail` and field-error payloads may be the safer contract choice until the team opts into migration.

## Validation Normalization

- preserve field-level detail
- preserve non-field errors
- do not leak stack traces or database internals

## Distinguish Clearly

- validation failure
- authentication failure
- permission denied
- not found
- conflict
- precondition required
- precondition failed
- throttled

## Implementation Rule

If clients need to branch on the error, the code must be stable and documented.
