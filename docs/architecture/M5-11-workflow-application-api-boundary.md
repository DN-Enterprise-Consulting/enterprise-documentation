# M5.11 – Workflow Application/API Boundary

## Purpose

M5.11 introduces the stable application boundary for workflow use cases. The application layer delegates to `PersistentWorkflowService` and does not construct persistence infrastructure.

## Boundary

```text
WorkflowApplication
        ↓
PersistentWorkflowService
        ↓
WorkflowRepository (SPI)
        ↓
Persistence adapter
```

`WorkflowRuntime` composes the infrastructure and exposes `WorkflowApplication` through `application()` while retaining `workflowService()` as a runtime-level compatibility accessor.

## Use cases

- create
- get
- findAll
- findByType
- findByStatus
- updateMetadata
- start
- complete
- fail

## Non-goals

- HTTP controllers
- REST DTOs
- authentication/authorization
- workflow orchestration across other modules
- external messaging

## Completion criterion

The complete Maven reactor must pass `mvn -B clean verify` with `BUILD SUCCESS`.
