# Permissions and Tenancy

## Principle

Authentication is transport-specific. Authorization is domain-specific.

## DRF Mapping

- Use authentication classes to establish caller identity.
- Use permission classes to gate access early.
- Use object-level checks for resource ownership, tenancy, or lifecycle constraints.

## Prefer

- reusable permission classes for common boundaries
- explicit object fetch plus scoped permission checks before mutation
- service-layer enforcement for critical invariants, even when views already check permissions

## Avoid

- role checks scattered through view logic
- fetching objects globally, then checking permissions after side effects start
- assuming `IsAuthenticated` is enough for directory, manager, or tenant-bound data

## Tenancy Guidance

- Scope querysets before serialization.
- Do not rely on client-supplied IDs alone when tenant boundaries matter.
- Make cross-tenant behavior explicit: deny, hide with `404`, or allow only for platform admins.

## Permission Review Checklist

- who can list?
- who can read one resource?
- who can mutate?
- who can perform transition actions?
- does the queryset already enforce scope?
- are object checks duplicated where they need to be?
