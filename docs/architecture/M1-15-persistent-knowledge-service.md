# M1.15 — Persistent Knowledge Service

## Purpose

M1.15 closes the end-to-end Knowledge persistence path by composing the
existing service logic with the repository and file persistence adapters.

Runtime path:

PersistentKnowledgeService
→ KnowledgeRepository
→ FileKnowledgeRepository
→ KnowledgePersistence
→ FileKnowledgePersistence
→ filesystem

## Responsibility

The service composes the repository abstraction and exposes the existing
Knowledge operations without depending on filesystem details.

Lifecycle state is persisted through the repository. Validation remains
covered by the domain model and the existing Knowledge validation layer.

## End-to-end verification

The integration tests verify:

1. creation is persisted
2. lifecycle state is persisted
3. content updates increment the patch version
4. a newly created service instance can reload the object
5. repository type queries see persisted data

## Non-goals

- Spring dependency injection
- database persistence
- transactions
- REST
- caching
- search indexing
- multi-process locking
- optimistic locking

## Definition of Done

- Persistent service is composed with `KnowledgeRepository`.
- Full Knowledge lifecycle state is persisted.
- Service recreation recovers persisted state.
- End-to-end integration tests pass.
- Existing Maven reactor remains green.
