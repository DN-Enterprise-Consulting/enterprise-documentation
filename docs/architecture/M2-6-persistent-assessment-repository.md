# M2.6 – Persistent Assessment Repository

## Purpose

M2.6 connects the assessment repository abstraction with the file-based persistence adapter introduced in M2.5.

The application-facing repository remains independent of filesystem details.

## Implementation

`FileAssessmentRepository` implements `AssessmentRepository` and delegates persistence operations to `AssessmentPersistence`.

Supported operations:

- save
- find by ID
- find all
- find by assessment type
- existence check

## Architecture

```text
Assessment Service
       |
       v
AssessmentRepository
       |
       v
FileAssessmentRepository
       |
       v
AssessmentPersistence
       |
       v
FileAssessmentPersistence
       |
       v
Filesystem
```

The repository does not depend directly on `java.nio.file` or on a concrete storage format.

## Design Rules

- repository depends on the persistence SPI
- no Spring dependency
- no database dependency
- no REST dependency
- no filesystem logic in the repository
- filtering remains deterministic and in-memory after persistence loading
- persistence remains replaceable

## Non-Goals

M2.6 does not introduce:

- database persistence
- transactions
- optimistic locking
- pagination
- full-text search
- REST endpoints
- Spring configuration
- caching

## Definition of Done

- `FileAssessmentRepository` implements `AssessmentRepository`
- persistence is delegated through `AssessmentPersistence`
- find-by-ID, find-all, type filtering and existence checks are covered by tests
- null contract is tested
- documentation is included
- full Maven reactor build must be green before M2.6 is considered complete
