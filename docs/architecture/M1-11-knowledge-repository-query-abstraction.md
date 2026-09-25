# M1.11 – Knowledge Repository Query Abstraction

## Purpose

M1.11 removes the remaining coupling between the query service and the in-memory repository implementation.

The application/query layer now depends exclusively on the `KnowledgeRepository` SPI.

## Architecture

```text
KnowledgeQueryService
        |
        v
KnowledgeRepository (SPI)
        |
        +----------------------+
        |                      |
        v                      v
InMemoryKnowledgeRepository   Future persistent repository
```

## Repository contract

The SPI now exposes:

- `save(...)`
- `findById(...)`
- `findByType(...)`
- `findAll()`
- `existsById(...)`

`findAll()` is the retrieval primitive required by the initial query service.

## Design consequence

`KnowledgeQueryService` no longer imports or stores a concrete repository implementation.

This makes the query layer replaceable independently from persistence technology.

## Tests

The milestone includes a repository abstraction test using a local SPI implementation rather than `InMemoryKnowledgeRepository`.

This verifies that the query service is coupled to the contract, not to the current storage implementation.

## Non-goals

M1.11 does not introduce:

- database persistence
- transactions
- pagination
- full-text search
- REST
- search engine integration

## Definition of Done

- Repository SPI exposes `findAll()`.
- In-memory repository implements the extended SPI.
- Query service depends only on the SPI.
- Contract tests cover the new operation.
- Query service abstraction test uses a non-in-memory repository implementation.
- Full Maven reactor remains green.
