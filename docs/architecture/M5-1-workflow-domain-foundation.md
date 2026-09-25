# M5.1 Workflow Domain Foundation

## Purpose

M5.1 establishes the framework-free domain foundation for Workflow.

The model is intentionally independent from persistence, runtime wiring,
REST, orchestration infrastructure, and external technologies.

## Domain Model

```text
WorkflowId
WorkflowType
WorkflowStatus
WorkflowMetadata
Workflow
```

## Workflow Types

- `ASSESSMENT`
- `KNOWLEDGE_PROCESSING`
- `RULE_EVALUATION`
- `PUBLICATION`

These values establish the initial domain vocabulary for workflow
instances. Additional types can be introduced by a later milestone when
there is a concrete orchestration use case.

## Initial Status Model

```text
DRAFT
RUNNING
COMPLETED
FAILED
```

M5.1 defines the status values only. Lifecycle transition rules are
deliberately deferred to the workflow lifecycle/service milestone.

## Domain Invariants

- `WorkflowId` requires a UUID.
- `Workflow` requires id, type, status and metadata.
- `WorkflowMetadata.name` must be non-blank.
- Description defaults to an empty string.
- Metadata attributes are immutable snapshots.
- `Workflow.draft(...)` creates a new workflow in `DRAFT`.

## Non-Goals

M5.1 does not introduce:

- persistence
- repositories
- workflow execution engines
- orchestration
- scheduling
- messaging
- REST/API adapters
- database dependencies
- Spring or other frameworks
