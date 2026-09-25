# M1.14 — Persistence Repository Integration

## Purpose

M1.14 connects the Knowledge repository abstraction with the concrete
file-based persistence implementation created in M1.13.

The intended runtime path is:

KnowledgeService
→ KnowledgeRepository
→ FileKnowledgeRepository
→ KnowledgePersistence
→ FileKnowledgePersistence
→ filesystem

## Responsibilities

`FileKnowledgeRepository` is an adapter between the repository SPI and the
persistence SPI.

It is responsible for:

- delegating save operations
- delegating ID lookup
- delegating existence checks
- deriving type filtering from repository-visible data
- exposing all persisted objects for the current file-backed implementation

It does not contain domain lifecycle or validation logic.

## Boundary

The repository depends on the technology-neutral `KnowledgePersistence`
contract rather than constructing filesystem access itself.

The concrete M1.13 persistence implementation remains replaceable.

## Current limitation

M1.12 defines only single-object persistence operations
(save/load/exists/delete). Repository-wide retrieval therefore requires the
file adapter to expose its own `loadAll()` capability.

This is intentionally documented as an M1.14 transitional boundary rather
than changing the M1.12 SPI retroactively.

A later milestone can introduce a technology-neutral query operation on the
persistence SPI when the requirements are sufficiently clear.

## Non-goals

M1.14 does not introduce:

- databases
- transactions
- ORM
- Spring integration
- REST
- caching
- search indexing
- pagination
- optimistic locking

## Definition of Done

- A concrete repository adapter exists.
- The adapter delegates persistence operations to `KnowledgePersistence`.
- Repository data survives recreation of the repository.
- `findAll()` and `findByType()` operate on persisted objects.
- Repository contract tests exist.
- Existing Maven reactor remains green.
