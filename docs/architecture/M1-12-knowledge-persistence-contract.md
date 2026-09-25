# M1.12 — Knowledge Persistence Contract

## Purpose

M1.12 establishes the persistence boundary for `KnowledgeObject`.

The goal is to define a technology-neutral SPI before introducing a concrete
database, ORM, filesystem or cloud persistence implementation.

## Contract

`KnowledgePersistence` provides four operations:

- `save(KnowledgeObject)` — stores or replaces the current representation.
- `load(KnowledgeObjectId)` — loads an object when it exists.
- `exists(KnowledgeObjectId)` — checks persistence existence.
- `delete(KnowledgeObjectId)` — removes the persisted representation.

## Architectural rules

1. The SPI lives in the `knowledge` module.
2. The SPI depends only on the knowledge domain.
3. No Spring, JPA, JDBC, SQL, database driver or cloud SDK is introduced.
4. Persistence technology remains behind the SPI.
5. Repository/query concerns remain separate from the persistence mechanism.
6. Implementations must preserve the complete `KnowledgeObject` domain state.

## Relationship to the repository

`KnowledgeRepository` is the application-facing repository abstraction used
for querying and aggregate access.

`KnowledgePersistence` is the technology-facing durable-storage boundary.

A future repository implementation may delegate to a `KnowledgePersistence`
implementation, but M1.12 intentionally does not introduce that adapter yet.

## Non-goals

M1.12 does not implement a database, JPA/Hibernate, transactions,
migrations, schema management, REST endpoints, search indexing, caching,
or optimistic locking.

## Definition of Done

- Persistence SPI exists.
- SPI is framework-free.
- Contract is covered by an executable test using a local stub.
- No concrete persistence technology is introduced.
- Architecture documentation is included.
- Existing repository/query abstractions remain unchanged.
