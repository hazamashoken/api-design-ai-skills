# View Selection

## Goal

Choose the smallest DRF surface that preserves the API contract clearly.

## Use Function Views When

- the endpoint is a narrow command or one-off action
- the behavior is simple enough that extra abstraction adds noise
- the operation is not conventional CRUD

## Use `APIView` When

- the endpoint has custom flow or branching logic
- several HTTP methods share domain context but not CRUD defaults
- you want explicit method handlers without viewset routing conventions

## Use `GenericAPIView` or Mixins When

- the endpoint is mostly conventional CRUD
- DRF generics reduce boilerplate without hiding behavior
- serializer and queryset hooks are still sufficient for clarity

## Use Viewsets When

- the resource model is stable and conventional
- router-generated URLs still match the intended contract
- custom actions do not overwhelm the main resource semantics

## Avoid

- viewsets that hide important permission or serializer branching
- action-heavy viewsets that behave like RPC controllers
- generic views when custom state transitions dominate behavior

## Rule of Thumb

Prefer the more explicit option when:

- auth is tricky
- read and write serializers differ substantially
- query validation matters
- lifecycle or transition endpoints exist
