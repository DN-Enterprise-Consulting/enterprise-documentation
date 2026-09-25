# M5.10 – Workflow Runtime Composition

## Purpose

M5.10 establishes the runtime composition boundary for the workflow module.

The runtime composes the existing file-backed persistence adapter, persistent repository and workflow service into one application-facing runtime object.

## Composition

```text
WorkflowRuntime
      |
      v
FileWorkflowPersistence
      |
      v
FileWorkflowRepository
      |
      v
PersistentWorkflowService
```

## Runtime API

`WorkflowRuntime.fileBacked(Path)` creates the complete file-backed workflow runtime.

`workflowService()` exposes the composed `PersistentWorkflowService` while keeping infrastructure construction inside the runtime boundary.

## Guarantees

- storage directory must not be null
- workflow data is persisted through the existing `.workflow` persistence format
- runtime recreation reloads persisted workflows
- no framework dependency is introduced
- no database dependency is introduced
- domain and SPI layers remain unchanged

## Verification

Run:

```bash
mvn -B clean verify
```

M5.10 is complete only after the complete Maven reactor reports `BUILD SUCCESS`.

## Correction

The hardening test is aligned with the actual `PersistentWorkflowService(WorkflowRepository)` constructor.
No second validator constructor is introduced solely for test compatibility.
