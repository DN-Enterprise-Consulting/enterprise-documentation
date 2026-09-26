# M8.10 Identity Runtime Composition

## Purpose

M8.10 defines the runtime composition boundary for the identity module.
The runtime composes file persistence, the persistent repository, validation,
the persistent identity service and query service without exposing
infrastructure construction to callers.

## Composition

`IdentityRuntime.fileBacked(Path)` creates:

1. `FileIdentityPersistence`
2. `FileIdentityRepository`
3. `IdentityValidator`
4. `PersistentIdentityService`
5. `IdentityQueryService`

All components share the same storage directory and repository instance.

## Public runtime accessors

- `persistence()`
- `repository()`
- `validator()`
- `identityService()`
- `queryService()`

## Guarantees

- null storage paths are rejected
- identities created through the runtime are persisted to `.identity` files
- runtime recreation reloads existing identities from the same storage
- lifecycle changes are visible through the query boundary
- no authentication, credentials, tokens or authorization policy are introduced

## Scope

M8.10 is composition only. The application/API boundary is M8.11.
