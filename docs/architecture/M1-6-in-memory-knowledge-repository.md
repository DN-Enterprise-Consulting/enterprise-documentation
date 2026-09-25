# M1.6 – In-Memory Knowledge Repository

## Purpose

M1.6 provides the first concrete implementation of the `KnowledgeRepository` SPI.

The implementation is intentionally infrastructure-light and uses an in-memory concurrent map. It provides a reference implementation for repository semantics and a deterministic basis for higher-level tests.

## Implementation

`InMemoryKnowledgeRepository` implements:

```text
KnowledgeRepository
       ▲
       │
InMemoryKnowledgeRepository
       │
       └── ConcurrentHashMap<KnowledgeObjectId, KnowledgeObject>
```

## Operations

- `save()` stores or replaces an object by its stable identifier.
- `findById()` returns the stored object or `Optional.empty()`.
- `findByType()` returns all stored objects matching the requested type.
- `existsById()` checks whether an identifier is present.
- `size()` is exposed for test/support purposes.

## Semantics

### Save

Saving an object with an existing `KnowledgeObjectId` replaces the previous object.

### Retrieval

`findById()` and `existsById()` use the object's stable identity.

`findByType()` filters the current repository state by `KnowledgeObjectType`.

### Null handling

Null repository arguments are rejected with `NullPointerException`.

## Concurrency

The backing store uses `ConcurrentHashMap`, making individual map operations safe for concurrent access. M1.6 does not define transactional or multi-operation consistency guarantees.

## Non-goals

M1.6 does not introduce:

- database persistence
- transactions
- caching
- search indexing
- pagination
- distributed storage
- Spring integration
- REST endpoints

## Definition of Done

- `InMemoryKnowledgeRepository` implements the M1.5 SPI.
- CRUD-like repository semantics required by the SPI are covered by tests.
- Replacement by stable identity is tested.
- Null argument handling is tested.
- No database or framework dependency is introduced.
- Maven reactor remains green.
