# M4.2 Publishing Repository Contract

## Purpose

Defines the persistence-independent repository boundary for `Publication`.

## Repository API

`PublicationRepository` exposes:

- `save(Publication)`
- `findById(PublicationId)`
- `findAll()`
- `findByType(PublicationType)`
- `findByStatus(PublicationStatus)`
- `existsById(PublicationId)`

## Contract Test

The contract test uses an in-memory stub only. No database, filesystem,
Spring, JPA, JDBC or external persistence technology is required.

The test stores publications keyed by their actual `PublicationId`. The
same ID returned by `save()` is used for `findById()` and `existsById()`.
This prevents the test from asserting against a different/random ID.

## Non-Goals

M4.2 does not introduce:

- database persistence
- file persistence
- ORM/JPA
- transactions
- REST/API adapters
- rendering
- publication workflow
