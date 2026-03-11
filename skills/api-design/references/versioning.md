# API Versioning And Evolution

## Start With Compatibility, Not Version Numbers

The first question is not "What version string should this API use?" The first question is "How can this contract evolve without breaking existing clients?"

Prefer:

- Additive fields
- Backward-compatible defaults
- Optional new inputs
- New endpoints, operations, or message types

Treat these as breaking:

- Removing fields
- Changing field meaning
- Tightening validation on previously accepted input
- Changing auth requirements without a migration plan
- Reusing names for new semantics

## Pick A Versioning Strategy Deliberately

Common options:

- Path versioning: `/v1/orders`
- Header versioning: `Accept: application/vnd.example.v1+json`
- Schema or contract versioning outside the URL with strong deprecation policy
- Message family or envelope versioning for WebSocket
- Field deprecation and additive evolution for GraphQL

Use path versioning when:

- Simplicity matters more than purity
- External consumers need obvious version boundaries
- Tooling and gateways expect explicit route segmentation

Use header or media-type versioning when:

- URLs should stay stable
- The organization already has strong API gateway and documentation support

Use GraphQL deprecation rather than endpoint versioning in most cases.

## Separate Breaking From Non-Breaking Changes

Document changes in these buckets:

- Safe immediately
- Safe with client awareness
- Requires migration
- Requires parallel support window

This keeps rollout planning grounded in client impact instead of abstract rules.

## Design Deprecation As A Process

For a breaking change:

1. Introduce the replacement first.
2. Mark the old contract deprecated with concrete guidance.
3. Communicate the timeline and migration steps.
4. Measure client usage.
5. Remove only after the agreed support window.

Good deprecation notices include:

- What is deprecated
- What replaces it
- When support ends
- What clients must change

## Keep Ownership And Canonical Paths Clear

If the system has REST plus GraphQL or WebSocket:

- State which interface owns writes for each business capability.
- Keep live-update channels additive to the canonical state interface.
- Avoid multiple partially overlapping versions of the same business mutation.

## Plan Rollouts Operationally

Versioning is not just syntax. Include:

- Client inventory
- Support window
- Monitoring and logging by version
- Migration aids
- Fallback or rollback plan

## Use Contract Tests And Examples

- Freeze important examples for old and new versions.
- Add compatibility tests around deprecated and replacement paths.
- Validate generated SDKs or clients when the spec changes.

## Review Checklist

- Could this change have been additive instead of versioned?
- Is the breaking surface area clearly enumerated?
- Is there a replacement path before deprecation?
- Is the support window explicit?
- Can usage of old versus new contracts be measured?
