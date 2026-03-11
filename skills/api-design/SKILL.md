---
name: api-design
description: Design and review application API contracts across RESTful HTTP, GraphQL, WebSocket, and OpenAPI-first workflows. Use when Codex needs to choose an API style, design endpoints, operations, schemas, or message types, model resources or events, define authentication, authorization, errors, idempotency, pagination, and async behavior, plan versioning and evolution, review an existing API contract, or explain tradeoffs between REST, GraphQL, WebSocket, and hybrid approaches.
---

# API Design

## Overview

Use this skill to design API contracts from product requirements, not just to format endpoints or schema fields. Start by choosing the right interaction model, then shape the contract, and only then write examples or implementation guidance.

## Triage The Task

Before designing, decide what kind of API work the user is asking for:

- For greenfield design, recommend the interaction style, model the domain, define the contract, and show concrete examples.
- For review work, lead with findings ordered by severity. Call out behavioral bugs, contract ambiguity, backward-compatibility risk, and missing operational semantics before offering rewrite suggestions.
- For evolution work, classify each proposed change as additive, compatibility-sensitive, or breaking. Then read `references/versioning.md`.
- For contract-first HTTP work, produce a generator-friendly OpenAPI outline and read `references/openapi.md`.
- For comparison work, evaluate at least domain fit, client ergonomics, operational complexity, and evolution cost before recommending a style.

## Route The Request

Choose the dominant interaction pattern before producing a design:

- Use REST when the system is resource-oriented, request-response driven, cacheable, and easy to model around nouns and state transitions. Then read `references/rest.md`.
- Use GraphQL when clients need flexible field selection, nested reads across aggregates, strong schema introspection, or a single graph contract for several UI surfaces. Then read `references/graphql.md`.
- Use WebSocket when the system needs long-lived connections, low-latency server push, bidirectional messaging, or session-oriented real-time flows. Then read `references/websocket.md`.
- Use a hybrid design only when the interaction modes are genuinely different. Keep one source of truth per capability and explain the boundary clearly.

When the boundary is ambiguous, prefer the simplest interface that fits:

- Prefer REST for canonical reads and writes over business resources.
- Prefer GraphQL for client-shaped read aggregation, not as a default write surface.
- Prefer WebSocket for delivery of live facts, session commands, or collaboration state, not general CRUD.
- Prefer OpenAPI when the team needs reviewable HTTP contracts, generated docs or SDKs, or governance before implementation.

## Follow The Design Workflow

1. Clarify the product capability.
   Capture actor, client type, latency sensitivity, consistency needs, and whether the problem is request-response, subscription, or conversational.
2. Model the domain.
   Define resources, aggregates, events, commands, and identities before naming routes or fields.
3. Choose the contract shape.
   Decide endpoint structure, schema types, or message envelopes based on the dominant interaction model.
4. Define cross-cutting rules.
   Cover authentication, authorization, errors, idempotency, pagination or streaming, async completion semantics, rate limits, observability, and evolution.
5. Produce concrete artifacts.
   Return example requests and responses, schemas, event payloads, and operational notes that an engineer can implement directly.

## Make Cross-Cutting Decisions Explicit

Do not leave these as implied defaults:

- Authentication mechanism and authorization boundary
- Error taxonomy, status or envelope semantics, and retry guidance
- Idempotency expectations for writes and reconnect or replay behavior where relevant
- Pagination, filtering, ordering, and consistency semantics for list or stream reads
- Async workflow semantics such as `202 Accepted`, job resources, callbacks, or eventual events
- Rate limits, quotas, or message throughput constraints
- Observability hooks such as request IDs, trace propagation, sequence numbers, or audit events
- Ownership when more than one interface exists for the same domain capability

## Apply Core Design Rules

- Prefer explicit domain language over transport-specific jargon.
- Separate reads from writes when that makes behavior clearer.
- Make errors machine-readable and stable.
- Prefer contract-first artifacts when the team needs reviews, generated docs, SDKs, or cross-team alignment before implementation.
- Design for evolution from the start: additive changes first, breaking changes only with an explicit migration plan.
- Keep ownership clear. Do not expose the same business mutation through several competing contracts without stating which one is canonical.
- Treat authentication and authorization as contract design, not an afterthought.
- Prefer a base structured error object with `code`, `status`, and `message`. Add `request_id` or `details` only when they materially help supportability or client handling.
- Standardize error codes in `UPPER_SNAKE_CASE` unless the surrounding system already has an established convention.
- Include examples that demonstrate the happy path and at least one failure path.

## Return Useful Deliverables

When asked to design or review an API, usually provide:

- A recommendation for REST, GraphQL, WebSocket, or a justified hybrid.
- The main contract artifacts: routes, schema, operations, events, or message types.
- Authentication and authorization boundaries.
- Error semantics, retry expectations, and idempotency rules.
- Pagination, filtering, subscription, ordering, or backpressure behavior where relevant.
- Versioning or schema evolution guidance.
- An OpenAPI outline when the HTTP contract should be specified before implementation.
- Short example interactions that make the contract concrete.

By transport, the minimum useful output usually looks like this:

- REST: resources, routes, methods, request and response shapes, status codes, errors, idempotency, pagination, and async job patterns if writes are not immediate.
- GraphQL: core types, query and mutation boundaries, input and payload types, pagination model, typed business errors, and deprecation path.
- WebSocket: connection lifecycle, auth timing, envelope shape, command and event catalog, delivery guarantees, ordering, backpressure, and close or reconnect behavior.
- Hybrid: explicit ownership matrix stating which interface is canonical for reads, writes, subscriptions, and notifications.

## Review Like A Contract Reviewer

When the user asks for a review, optimize for defect detection rather than redesign prose:

- Lead with the most serious issues first.
- Call out concrete risk: incorrect semantics, hidden breaking changes, missing auth boundaries, inconsistent errors, undefined ordering, weak idempotency, or unclear ownership.
- Cite the exact route, field, operation, or message type that causes the issue.
- Separate required changes from optional polish.
- If the design is acceptable, say so explicitly and list any residual risk or testing gaps.

## Load References Selectively

- Read `references/rest.md` for resource modeling, HTTP semantics, status codes, idempotency, and versioning.
- Read `references/graphql.md` for schema design, query and mutation boundaries, pagination, errors, and subscriptions.
- Read `references/websocket.md` for connection lifecycle, message envelopes, ordering, retries, and real-time operational concerns.
- Read `references/openapi.md` when the task calls for contract-first HTTP design, OpenAPI structure, reusable components, or generator-friendly REST contracts.
- Read `references/versioning.md` when the user asks about backward compatibility, deprecation, rollout strategy, or breaking-change management.
- For comparisons or hybrid recommendations, skim all three references but load deeply only the chosen transport styles.

## Reuse Templates When Helpful

- Use `assets/api-design-rfc-template.md` when the user wants a design proposal, technical RFC, or review-ready API decision document.
- Use `assets/api-review-template.md` when the user wants a structured review of an existing API design or schema.
