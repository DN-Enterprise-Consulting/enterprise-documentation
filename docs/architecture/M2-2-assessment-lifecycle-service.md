# M2.2 – Assessment Lifecycle & Service

## Purpose

M2.2 establishes the application/service layer for the Assessment domain and defines its lifecycle transitions.

## Lifecycle

```text
DRAFT -> IN_PROGRESS -> COMPLETED -> ARCHIVED
```

Only the next lifecycle state is reachable. `ARCHIVED` is terminal.

## Components

### AssessmentLifecycleService

Responsible for validating and applying lifecycle transitions.

Allowed transitions:

| Current | Target |
|---|---|
| DRAFT | IN_PROGRESS |
| IN_PROGRESS | COMPLETED |
| COMPLETED | ARCHIVED |

Same-state transitions are idempotent.

All other transitions raise `AssessmentLifecycleException`.

### AssessmentService

Provides the application-level operations:

- create
- get
- updateMetadata
- start
- complete
- archive

The service remains framework-free and has no persistence, REST or database dependency.

## Update rules

Metadata can be updated while an assessment is not archived.

The assessment identity and lifecycle status remain unchanged during a metadata update.

Archived assessments are immutable through the service API.

## Non-goals

M2.2 does not introduce:

- persistence
- repositories
- REST controllers
- Spring
- authentication/authorization
- assessment execution logic
- scoring
- reporting
- workflow orchestration

## Dependencies

The implementation remains within the `assessment` module and uses the existing assessment domain model from M2.1.

## Definition of Done

- lifecycle transitions explicitly modeled
- invalid transitions rejected
- archived state terminal
- application service introduced
- metadata update rule implemented
- unit tests cover valid and invalid transitions
- architecture documentation added
- no infrastructure dependency introduced
