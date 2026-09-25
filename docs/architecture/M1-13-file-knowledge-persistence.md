# M1.13 — File Knowledge Persistence

## Purpose

M1.13 provides the first concrete implementation of the M1.12
`KnowledgePersistence` contract.

The implementation uses the local filesystem and deliberately introduces no
database, ORM, Spring integration or third-party serialization framework.

## Implementation

`FileKnowledgePersistence` stores one `KnowledgeObject` per file.

- Root directory is supplied through the constructor.
- File name is derived from the immutable `KnowledgeObjectId`.
- UTF-8 is used for persistence.
- String fields are Base64 encoded so line-oriented storage remains robust
  for newlines, separators and Unicode.
- Metadata attributes are persisted in deterministic key order.

## Durability

A second `FileKnowledgePersistence` instance can load data written by the
first instance. This demonstrates persistence beyond the lifetime of the
Java object itself.

## Error handling

Filesystem and malformed-record failures are translated into the
technology-local `KnowledgePersistenceException`.

Missing objects are represented by `Optional.empty()`.

## Non-goals

M1.13 does not implement:

- database persistence
- transactions
- concurrent write coordination
- file locking
- schema migration
- encryption
- compression
- REST APIs
- repository integration

These remain separate concerns for later milestones.

## Definition of Done

- Concrete `KnowledgePersistence` implementation exists.
- Data survives recreation of the persistence object.
- Full `KnowledgeObject` state is round-tripped.
- Delete and missing-object behavior are covered.
- Root directory creation is covered.
- Existing Maven reactor remains framework-free.
