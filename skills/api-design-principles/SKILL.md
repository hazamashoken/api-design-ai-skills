---
name: api-design-principles
description: Framework-agnostic API design doctrine for REST, GraphQL, WebSocket, and OpenAPI work. Use for resource modeling, URL design, versioning, pagination, filtering, error contracts, auth boundaries, idempotency, naming, backward compatibility, and contract review before framework-specific implementation.
---

# API Design Principles

Use this skill for the durable part of API work: the contract decisions that
should survive framework changes.

Do not start from DRF, FastAPI, Express, Spring, or Rails mechanics. Start from
the product capability, the domain model, and the contract semantics.

## Use This Skill For

- Choosing between REST, GraphQL, WebSocket, or a justified hybrid
- Resource modeling and URL design
- Query model design: filtering, sorting, pagination, search
- Error format and status semantics
- Authentication and authorization boundaries
- Idempotency, concurrency, retries, and async workflow semantics
- Versioning, deprecation, and backward-compatibility planning
- OpenAPI-first design and contract review
- API testing expectations and review criteria

## Do Not Use This Skill Alone When

- The main question is how to implement the chosen design in one framework
- The user explicitly wants DRF-specific serializer, permission, pagination,
  exception-handler, or schema guidance

In those cases, read `../api-design-drf/SKILL.md` after the design is clear.

## Core Doctrine

- API design is architecture first, framework syntax second.
- Prefer domain language over controller or transport jargon.
- Treat authentication, authorization, idempotency, and errors as contract design.
- Preserve established contracts unless the task explicitly calls for redesign or migration.
- When a project already relies on framework-default behavior, treat those defaults as part of
  the current contract until the team chooses to change them.
- Prefer additive evolution first. Treat required new fields, meaning changes,
  and removals as breaking until proven otherwise.
- Keep one canonical contract per business capability. If several interfaces
  exist, make ownership explicit.

## Design Workflow

1. Clarify the product capability.
   Capture actor, client type, consistency needs, latency expectations, and
   whether the interaction is request-response, subscription, or conversational.
2. Model the domain.
   Define resources, aggregates, events, commands, and identifiers before naming routes.
3. Choose the interaction style.
   Pick REST, GraphQL, WebSocket, or hybrid based on domain fit and actual stack support, not habit.
4. Define the contract.
   Specify routes or operations, payloads, errors, auth, idempotency, pagination,
   ordering, async behavior, and observability hooks.
5. Plan evolution.
   Classify changes as additive, compatibility-sensitive, or breaking.
6. Only then map the contract into framework-specific implementation notes.

## Transport Selection

- Prefer REST for canonical reads and writes over business resources.
- Prefer GraphQL for client-shaped reads across aggregates, not as the default write surface.
- Prefer WebSocket for live facts, low-latency session flows, or bidirectional collaboration.
- Prefer OpenAPI when the team needs reviewable contracts, generated docs, SDKs, or governance.
- Do not recommend a transport the project does not support unless the user is explicitly exploring
  a platform change.

## Cross-Cutting Rules

Always make these explicit:

- authentication mechanism
- authorization boundary
- tenancy and isolation boundary
- error envelope and retry guidance
- pagination, filtering, sorting, and ordering rules
- idempotency expectations for unsafe writes
- concurrency controls where updates can race
- async completion semantics for non-immediate work
- rate limits or throughput constraints
- request IDs, trace propagation, or audit hooks
- versioning and deprecation policy

## Review Mode

When reviewing an existing API, lead with findings ordered by severity.

Prioritize:

- incorrect semantics
- hidden breaking changes
- missing auth boundaries
- tenancy or isolation leaks
- inconsistent errors
- undefined ordering or pagination behavior
- weak idempotency or retry semantics
- unclear ownership between transports

Keep redesign suggestions secondary to defect detection.

## Return Useful Deliverables

Usually return:

- a transport recommendation
- the main contract artifacts: routes, operations, events, or message types
- auth and authorization rules
- error semantics and retry expectations
- pagination, filtering, ordering, or streaming semantics
- versioning and migration guidance
- OpenAPI outline when HTTP contract-first work is needed
- happy-path and failure examples

## Testing Expectations

The design is not done until the contract is testable.

State:

- which behaviors need contract tests
- which error cases must be covered
- how pagination and filtering should be verified
- how backward compatibility will be protected
- how auth and tenancy boundaries will be exercised

## Load References Selectively

- Read `references/rest.md` for resource modeling, status codes, idempotency,
  pagination, and review criteria.
- Read `references/graphql.md` for schema design, query and mutation boundaries,
  pagination, typed errors, and subscriptions.
- Read `references/websocket.md` for connection lifecycle, envelopes, ordering,
  retries, and delivery guarantees.
- Read `references/openapi.md` for contract-first HTTP design and reusable schema patterns.
- Read `references/versioning.md` when evolution, deprecation, and rollout strategy matter.

## Reuse Templates When Helpful

- Use `assets/api-design-rfc-template.md` for design proposals or RFCs.
- Use `assets/api-review-template.md` for structured API reviews.
