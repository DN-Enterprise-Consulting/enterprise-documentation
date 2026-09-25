# M2.5 – File-Based Assessment Persistence

## Purpose

M2.5 introduces the first concrete implementation of the `AssessmentPersistence` SPI.

## Storage model

Each assessment is stored as one UTF-8 text file:

```text
<assessment-id>.assessment
```

The file contains six deterministic lines:

1. Assessment ID
2. Assessment type
3. Assessment status
4. Metadata name
5. Metadata description
6. Metadata attributes

String values are Base64 encoded. Metadata attributes are written in sorted key order.

## Behaviour

- storage directory is created on demand
- `save` writes or replaces the complete assessment state
- `load` reconstructs the persisted assessment
- `loadAll` returns all `.assessment` files
- `exists` checks for the corresponding file
- `delete` removes the corresponding file
- UTF-8 is used consistently
- no external serialization library is required

## Architecture

```text
AssessmentRepository
        |
        v
AssessmentPersistence
        |
        v
FileAssessmentPersistence
        |
        v
filesystem
```

M2.5 does not yet change `AssessmentRepository` to use this implementation. Repository integration is a separate step.

## Non-goals

No:

- Spring
- JPA/Hibernate
- JDBC
- SQL
- database
- external serializer
- REST
- search engine
- transaction management
- optimistic locking

## Definition of Done

- concrete file persistence implementation
- deterministic file representation
- save/load round-trip
- loadAll
- exists/delete
- metadata and lifecycle status preserved
- error boundary introduced
- unit tests added
- architecture documentation added
