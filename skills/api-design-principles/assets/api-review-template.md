# API Design Review

## Scope

- Interface under review:
- Change type:
- Intended consumers:
- Canonical source of truth:

## Findings

List issues ordered by severity with direct references to the contract where possible. Focus on bugs, ambiguity, backward-compatibility risk, and missing operational semantics before style feedback.

## Verdict

- Ship as-is:
- Blocking issues:
- Main migration or rollout risk:

## Domain Fit

- Does the contract reflect the business model?
- Are resources, types, commands, and events named clearly?

## Protocol Fit

- Is REST, GraphQL, WebSocket, or a hybrid justified?
- Are responsibilities separated cleanly across interfaces?

## Contract Quality

- Are inputs and outputs explicit?
- Are errors stable and machine-readable?
- Are auth and authorization boundaries clear?
- Are idempotency, ordering, and retry semantics defined where needed?
- Are async workflows explicit when writes are not immediate?

## Evolution

- Is the change backward compatible?
- If not, is the migration path explicit?
- Are deprecations and support windows defined?

## Operational Risks

- Performance:
- Scalability:
- Observability:
- Abuse resistance:
- Client upgrade risk:

## Required Changes

- 

## Suggested Improvements

- 
