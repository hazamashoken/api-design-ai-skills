# Schema and drf-spectacular

## Goal

Keep implementation and contract documentation aligned.

## Use drf-spectacular For

- operation summaries and descriptions
- query parameters
- request bodies
- success responses
- reusable error responses
- examples for both success and failure

## Preferred Patterns

- stable `operationId` naming
- shared pagination envelope components
- shared error response components
- explicit request vs response serializers when shapes differ

## Avoid

- relying on serializer inference when the contract is more specific than the model
- undocumented headers such as `ETag`, `If-Match`, or rate-limit metadata
- publishing endpoints before the schema reflects actual behavior

## Review Checklist

- do documented status codes match runtime behavior?
- do examples cover failure paths?
- are auth requirements explicit?
- are query params documented with defaults and limits?
