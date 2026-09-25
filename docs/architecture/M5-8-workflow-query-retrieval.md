# M5.8 – Workflow Query / Retrieval

## Purpose

M5.8 introduces a dedicated query model and query service for workflow retrieval.

## Components

- `WorkflowQuery` – immutable query criteria for workflow type and/or status.
- `WorkflowQueryService` – application-facing retrieval service using only `WorkflowRepository`.

## Query modes

- `all()` – no filters
- `byType(type)` – filter by workflow type
- `byStatus(status)` – filter by workflow status
- `byTypeAndStatus(type, status)` – combine both filters

When both criteria are present, they are combined using AND semantics.

## Boundary

`WorkflowQueryService` depends only on `WorkflowRepository`. It does not depend on
`InMemoryWorkflowRepository`, `FileWorkflowRepository`, or `WorkflowPersistence`.

The repository remains the persistence/retrieval abstraction; concrete storage is
selected outside the query layer.

## Scope / Non-Goals

M5.8 does not introduce pagination, sorting, full-text search, persistence changes,
or runtime composition. Those concerns remain outside this milestone.
