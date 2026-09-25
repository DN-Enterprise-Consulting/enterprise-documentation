# M4.4 Publication Persistence Contract

## Purpose

M4.4 defines the persistence boundary for `Publication`.

The contract is intentionally independent of any concrete persistence
technology. It allows later repository implementations to delegate durable
storage without coupling the publication domain to files, databases, ORM
frameworks or external services.

## SPI

`PublicationPersistence` exposes:

- `save(Publication)`
- `load(PublicationId)`
- `exists(PublicationId)`
- `delete(PublicationId)`

## Contract Semantics

`save()` returns the saved publication.

`load()` returns an `Optional` and is empty when no publication exists for the
requested identifier.

`exists()` reports whether the identifier is present.

`delete()` removes the publication for the identifier. Deleting a missing
identifier is allowed to be a no-op.

## Test Strategy

The contract test uses a local in-memory stub. No database, filesystem,
Spring, JPA, JDBC, serializer or cloud SDK is required.

## Non-Goals

M4.4 does not introduce:

- file persistence
- database schema
- ORM/JPA
- transactions
- migrations
- REST
- search
- caching
- publication rendering
- lifecycle orchestration
