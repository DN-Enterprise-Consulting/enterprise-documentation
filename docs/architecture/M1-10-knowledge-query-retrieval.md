# M1.10 – Knowledge Query & Retrieval

## Purpose

M1.10 introduces a small, framework-free query layer for Knowledge Objects.

The first query dimensions are:

- Knowledge Object type
- lifecycle status
- combination of type and status

## Query model

`KnowledgeQuery` is an immutable query object.

Supported factories:

- `all()`
- `byType(type)`
- `byStatus(status)`
- `byTypeAndStatus(type, status)`

## Query flow

```text
Caller
  |
  v
KnowledgeQuery
  |
  v
KnowledgeQueryService
  |
  v
InMemoryKnowledgeRepository
```

The query service applies all supplied criteria as an AND combination.

## Repository extension

The in-memory repository exposes `findAll()` for the query layer.

This is intentionally an in-memory capability. No database or search engine is introduced in M1.10.

## Non-goals

M1.10 does not introduce:

- full-text search
- ranking
- relevance scoring
- pagination
- REST endpoints
- database persistence
- external search engines
- authorization

## Design rule

`KnowledgeQueryService` is deliberately independent of REST and persistence technology.

The query model is also separate from the repository SPI so that a later persistence implementation can provide an equivalent retrieval capability without changing the caller-facing query semantics.

## Definition of Done

- Query object implemented.
- Queries for all/type/status/type+status supported.
- Query service implemented.
- In-memory retrieval supported.
- Tests cover matching and empty results.
- No framework dependency introduced.
