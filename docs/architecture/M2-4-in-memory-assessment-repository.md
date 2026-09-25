# M2.4 – In-Memory Assessment Repository

## Purpose

M2.4 provides the first concrete implementation of the `AssessmentRepository` SPI.

## Implementation

`InMemoryAssessmentRepository` stores assessments in a thread-safe `ConcurrentHashMap` keyed by `AssessmentId`.

Supported operations:

- save
- findById
- findAll
- findByType
- findByStatus
- existsById

An additional `size()` method is provided for test/support purposes.

## Behaviour

Saving an assessment with an existing `AssessmentId` replaces the previous value.

Queries return immutable stream results through Java's `Stream.toList()`.

Null arguments are rejected explicitly.

## Architecture

```text
AssessmentService
       |
       v
AssessmentRepository
       ^
       |
InMemoryAssessmentRepository
```

The implementation does not modify the Assessment domain or lifecycle model.

## Non-goals

M2.4 does not introduce:

- database persistence
- filesystem persistence
- Spring
- JPA/Hibernate
- JDBC
- transactions
- REST
- search infrastructure
- caching

The implementation is intended as a deterministic repository for tests, local runtime composition and the next persistence integration step.

## Definition of Done

- concrete repository implements `AssessmentRepository`
- save/find operations implemented
- type/status filtering implemented
- replacement by identifier implemented
- null handling tested
- repository behaviour covered by unit tests
- no infrastructure dependency introduced
- architecture documentation added
