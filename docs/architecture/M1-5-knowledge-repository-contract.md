# M1.5 – Knowledge Repository Contract

## Purpose

M1.5 defines the persistence-independent repository contract for Knowledge Objects.

The contract separates the Knowledge domain from storage technology. Concrete implementations can be introduced later without changing the domain model or the repository SPI.

## Contract

`KnowledgeRepository` provides:

- `save(KnowledgeObject)`
- `findById(KnowledgeObjectId)`
- `findByType(KnowledgeObjectType)`
- `existsById(KnowledgeObjectId)`

## Design rules

- The SPI depends only on Knowledge domain types and Java standard library types.
- Retrieval by identifier returns `Optional`.
- Type-based retrieval returns a `List`.
- Existence checks are explicit.
- No persistence technology is referenced.
- No Spring, JPA, JDBC, Elasticsearch or REST types are exposed.

## Architectural position

```text
Knowledge Domain
       |
       v
KnowledgeRepository (SPI)
       |
       +---- future implementation(s)
```

The concrete storage implementation is intentionally outside M1.5.

## Non-goals

M1.5 does not define:

- database schema
- transactions
- caching
- search indexing
- pagination
- authorization
- REST endpoints
- a concrete repository implementation

These concerns belong to later increments.

## Definition of Done

- Repository interface implemented.
- All operations use domain types.
- Contract test verifies the exposed API.
- No infrastructure dependency introduced.
- Maven reactor remains green.
