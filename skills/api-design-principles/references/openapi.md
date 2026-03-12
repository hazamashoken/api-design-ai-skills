# OpenAPI-First Design

## Use OpenAPI First When

- The API is HTTP-based and needs a reviewable contract before implementation.
- Several teams or external integrators depend on stable generated docs or SDKs.
- Governance, linting, mocking, or contract testing should happen before code exists.
- The team needs a single source of truth for routes, parameters, payloads, and errors.

OpenAPI is strongest for RESTful HTTP contracts. It is not the right abstraction for GraphQL schemas or native WebSocket message catalogs.

## Start From The Domain, Then Encode The Contract

Before writing the spec, define:

- Resources and identifiers
- Operations and state transitions
- Authentication model
- Error taxonomy
- Pagination and filtering rules
- Versioning approach

Then express those decisions in OpenAPI instead of designing directly inside the YAML.

If the API already exists, describe the contract that clients actually use before proposing a cleaner
replacement. OpenAPI should document the current canonical behavior unless the task is explicitly to
redesign and migrate it.

## Prefer A Stable Spec Structure

Keep the document organized around:

- `openapi`
- `info`
- `servers`
- `tags`
- `paths`
- `components`
- `security`

Use `components` aggressively for reuse:

- `schemas`
- `parameters`
- `responses`
- `requestBodies`
- `headers`
- `securitySchemes`

## Design Operations For Humans And Tools

- Give each operation a clear `summary`.
- Use stable, meaningful `operationId` values because generators depend on them.
- Tag operations consistently by domain capability, not by implementation layer.
- Keep parameter names and schema names predictable.

Good `operationId` examples:

- `listOrders`
- `getOrder`
- `createOrder`
- `cancelOrder`

Avoid opaque names such as `ordersControllerGetAll`.

## Use Schemas Deliberately

- Prefer explicit object schemas over free-form blobs.
- Mark required fields carefully.
- Add format hints only when they are accurate and useful.
- Use enums for bounded sets of values.
- Keep read-only and write-only semantics explicit.

When request and response shapes diverge, model them separately rather than forcing one schema to do both.

## Standardize Errors

Define reusable error responses in `components.responses` and shared error schemas in `components.schemas`.

Useful shared patterns:

- Validation error
- Authentication error
- Authorization error
- Not found
- Conflict
- Rate limit

Keep the shared error schema centered on `code`, `status`, and `message`. Keep error codes stable, machine-readable, and in `UPPER_SNAKE_CASE`. Add optional fields such as `request_id` and `details` only when they are intentionally part of the contract.

If an existing API already relies on framework-default error payloads, model that faithfully in the
spec rather than silently imposing a new shared envelope.

## Model Security Explicitly

- Define security schemes centrally.
- Apply security globally only when most operations share it.
- Override at the operation level where capabilities differ.
- Document required scopes or permissions close to the operation or in extension fields if your tooling supports that.

## Make The Spec Generator-Friendly

- Avoid ambiguous polymorphism unless consumers truly need it.
- Keep schema reuse intentional and understandable.
- Prefer additive schema changes.
- Be careful with `oneOf`, `anyOf`, and `allOf`; use them only when the contract semantics are clear.
- Preserve established path structure and parameter conventions unless the migration plan is part of the task.

## Design Examples Into The Spec

Include examples for:

- Representative requests
- Representative successful responses
- At least one common error response

Examples improve review quality and generated docs.

## Review Checklist

- Does the spec express an already-clear API design rather than inventing one ad hoc?
- Are operation IDs stable and human-readable?
- Are schemas reusable without becoming vague?
- Are auth and error behaviors explicit?
- Can generators and docs consumers use the spec without manual interpretation?
