# Serializer Patterns

## Query Serializers

- Validate query params with serializers when parameters affect filtering, pagination,
  ordering, projection, or revision selection.
- Prefer explicit serializer fields over ad hoc `request.query_params.get(...)` parsing.

## Read and Write Separation

- Use separate serializers when response shape differs materially from write shape.
- Keep server-generated fields read-only.
- Avoid forcing one serializer to serve create, update, and read unless the model is trivial.

## Partial Updates

- Use `PATCH` semantics deliberately.
- Validate only the fields the contract allows to change.
- Be explicit about merge behavior for nested objects or JSON blobs.

## Nested Structures

- Use nested serializers when the contract truly nests resources or structured fields.
- Avoid deep writable nesting unless the business transaction is genuinely atomic and stable.

## ModelSerializer Caution

- `ModelSerializer` is fine for internal speed, but do not let it leak fields by accident.
- Audit exposed fields, read-only fields, and validation messages before treating it as public API.

## Validation Shape

- Normalize serializer validation output when clients need a stable machine-readable error format.
- Keep field-level and non-field validation distinct.
