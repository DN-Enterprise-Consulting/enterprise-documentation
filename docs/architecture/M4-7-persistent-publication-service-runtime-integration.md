# M4.7 Persistent Publication Service & Runtime Integration

## Purpose

M4.7 introduces the application/service boundary for publications and
connects it to the persistent repository and file-backed runtime.

## Runtime Composition

```text
PublicationRuntime
        |
        v
PersistentPublicationService
        |
        v
PublicationRepository
        |
        v
FilePublicationRepository
        |
        v
FilePublicationPersistence
        |
        v
*.publication
```

The service is repository-driven and does not depend directly on the file
implementation. The runtime is the composition boundary that wires the
concrete file-backed implementation.

## Service Operations

- create
- get
- findAll
- findByType
- findByStatus
- updateMetadata
- publish
- archive

Publication lifecycle is enforced as:

`DRAFT -> PUBLISHED -> ARCHIVED`

`ARCHIVED` is terminal. Archived publications cannot be modified.

## Integration Tests

The tests cover:

- service creation and retrieval
- metadata update
- publication and archival
- invalid lifecycle transitions
- archived-object modification protection
- missing publication handling
- persistence across runtime recreation
- concrete file-backed runtime composition

## Non-Goals

M4.7 does not introduce:

- REST endpoints
- rendering
- templates
- document generation
- databases
- transactions
- search
- caching
