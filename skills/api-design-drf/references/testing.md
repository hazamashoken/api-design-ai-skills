# Testing the DRF Contract

## Goal

Test the public contract, not just internal code paths.

## Minimum Coverage

- happy path
- invalid input
- auth failure
- permission denial
- not found
- pagination and filtering behavior
- ordering guarantees
- idempotent retry behavior where promised
- concurrent write or precondition handling where relevant

## Serializer Tests

- verify boundary validation rules
- verify field-level and non-field error shape if the contract depends on it

## View Tests

- assert both status codes and response bodies
- verify permission and tenancy boundaries explicitly
- verify headers such as `ETag`, pagination links, or rate-limit metadata when part of the contract

## Schema Tests

- protect critical schema generation for contract-first endpoints
- catch drift between implementation and published docs

## Regression Strategy

- add tests for every fixed contract bug
- add migration-period tests if old and new endpoints coexist temporarily
