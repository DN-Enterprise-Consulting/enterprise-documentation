# M4.6 Persistent Publication Repository Integration

## Purpose

M4.6 connects the `PublicationRepository` abstraction with the concrete
file-based persistence implementation.

## Composition

```text
PublicationRepository
        |
        v
FilePublicationRepository
        |
        v
PublicationPersistence
        |
        v
FilePublicationPersistence
        |
        v
*.publication
```

`FilePublicationRepository` delegates single-object operations to the
`PublicationPersistence` SPI.

For `findAll()`, `findByType()` and `findByStatus()`, the current
`PublicationPersistence` SPI does not yet expose a `loadAll()` operation.
Therefore M4.6 uses the same transitional concrete-type boundary already
used by the earlier repository integrations:

`FilePublicationPersistence` provides `loadAll()`, and the repository
requires that concrete implementation for collection queries.

A future persistence-query SPI can remove this transitional dependency.

## Integration Tests

The tests verify:

- save/load through repository and file persistence
- existence checks
- loading all persisted publications
- type filtering
- status filtering
- repository recreation against existing files
- missing objects
- null validation
- explicit behavior when a persistence implementation does not provide
  `loadAll()`

## Non-Goals

M4.6 does not introduce:

- databases
- JPA/JDBC
- transactions
- REST
- rendering
- caching
- search infrastructure
