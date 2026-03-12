# GraphQL API Design

## Use GraphQL When

- Clients need different field sets over the same domain graph.
- UIs need to compose data across aggregates without endpoint proliferation.
- Strong typed introspection and schema-first evolution are valuable.
- Several clients share a graph but differ in query depth and shape.

Do not choose GraphQL just to avoid designing clear domain boundaries.

## Design The Schema Around The Domain

- Start from core types, relationships, and identities.
- Make object types represent stable business concepts.
- Use fields to express relationships, not transport shortcuts.
- Keep naming consistent and predictable across the schema.

Prefer:

- `type Order { id, status, customer, lineItems }`
- `type Query { order(id: ID!): Order }`

Avoid:

- Types that mirror database joins or UI widgets directly
- Generic blobs that weaken type safety

## Separate Query And Mutation Responsibilities

- Keep queries side-effect free.
- Use mutations for state change, commands, or workflows.
- Name mutations after business actions when they are not simple updates.
- Return payload types from mutations instead of raw scalars.

Example:

```graphql
type Mutation {
  cancelOrder(input: CancelOrderInput!): CancelOrderPayload!
}
```

Payloads should usually include:

- The affected object or edge
- Domain errors or result metadata
- Client correlation data when useful

## Design Inputs Carefully

- Use input objects for non-trivial arguments.
- Keep required fields explicit.
- Avoid highly nested write inputs unless the transactional model is well defined.
- Use enums for bounded values and document forward-compatibility expectations.

## Control Query Cost

- Define limits for depth, breadth, and expensive field fan-out.
- Prefer pagination on collections by default.
- Document complexity-sensitive fields.
- Avoid unbounded nested lists.

If a field is expensive, say so and consider:

- Pagination
- Dedicated arguments
- Separate fields for summary versus full detail

## Use Relay-Style Concepts When Helpful

Use connections for large or mutable lists:

- `edges`
- `node`
- `pageInfo`
- cursors

Cursor pagination is usually better than offset pagination for dynamic datasets.

## Handle Errors Predictably

- Reserve top-level GraphQL errors for transport, parsing, validation, or unexpected execution failures.
- Put expected business failures in typed payloads when clients should handle them deliberately.
- Keep the core typed error fields as `code`, `status`, and `message`.
- Keep error codes stable, machine-readable, and in `UPPER_SNAKE_CASE`.

Example pattern:

```graphql
type CancelOrderPayload {
  order: Order
  userError: UserError
}

type UserError {
  code: String!
  status: String!
  message: String!
}
```

Example `UserError.code` values:

- `ORDER_STATE_CONFLICT`
- `INVALID_SORT_FIELD`

Optional fields such as `requestId` or `details` are useful when clients need support correlation or structured recovery data, but keep the base error shape stable first.

## Design Authentication And Authorization

- Authenticate once at the transport boundary.
- Authorize per field, object, or mutation as needed.
- Avoid leaking hidden data through partial nulls without explanation.
- Be consistent about whether unauthorized access yields `null`, typed error payloads, or execution errors.

## Plan Schema Evolution

- Add new fields and types freely when backward compatible.
- Deprecate before removing.
- Keep old fields functioning during the migration window.
- Do not repurpose fields with new semantics.

When changing behavior:

- Add new fields first
- Mark old fields deprecated with guidance
- Remove only after a communicated deprecation period

## Treat N+1 As A Design Concern

- Expect relational field resolution to need batching.
- Design resolvers and backing services for predictable access patterns.
- Avoid schemas that force repeated per-node backend calls for common views.

## Use Subscriptions Sparingly

- Use subscriptions for true real-time updates, not general polling replacement.
- Define event granularity clearly.
- State delivery guarantees, ordering expectations, and reconnect behavior.
- If subscriptions are important, compare them with a dedicated WebSocket contract in `websocket.md`.

## Review Checklist

- Does the schema reflect domain concepts instead of storage details?
- Are queries side-effect free and mutations explicit?
- Are connections paginated and bounded?
- Are business errors typed rather than hidden in generic execution failures?
- Is schema evolution additive and deliberate?
- Are expensive fields and resolver patterns under control?
