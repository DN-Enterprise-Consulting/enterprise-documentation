# M5.7 – Workflow Service & Lifecycle

## Scope

M5.7 adds the application-facing workflow service on top of `WorkflowRepository` and defines the first explicit workflow lifecycle rules.

## Service

`PersistentWorkflowService` provides:

- create
- get
- findAll
- findByType
- findByStatus
- updateMetadata
- start
- complete
- fail

The service depends only on the workflow repository SPI.

## Lifecycle

The implemented transitions are:

```text
DRAFT -> RUNNING -> COMPLETED
                 -> FAILED
```

`COMPLETED` and `FAILED` are terminal states for this milestone. Metadata changes are allowed only while a workflow is `DRAFT`.

Invalid transitions raise `WorkflowLifecycleException`. Missing workflows raise `WorkflowNotFoundException`.

## Non-goals

- workflow execution engine
- retries
- orchestration between modules
- scheduling
- API/web endpoints
- validation rules beyond existing domain invariants
