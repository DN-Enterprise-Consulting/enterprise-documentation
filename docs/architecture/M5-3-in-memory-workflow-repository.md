# M5.3 – In-Memory Workflow Repository

## Purpose

M5.3 provides the first concrete implementation of the `WorkflowRepository` SPI without introducing persistence or framework dependencies.

## Implementation

`InMemoryWorkflowRepository` uses a `ConcurrentHashMap<WorkflowId, Workflow>` as its internal store.

Supported operations:

- save and replace by workflow ID
- find by ID
- find all
- filter by workflow type
- filter by workflow status
- existence check by ID

The implementation returns the workflow passed to `save` and keeps the repository independent from infrastructure technologies.

## Concurrency

The underlying `ConcurrentHashMap` provides thread-safe individual repository operations. Query results are returned as immutable list snapshots via `List.copyOf` / stream collection.

## Validation

The repository rejects null workflow and query arguments with `NullPointerException`. Domain validation remains owned by the workflow domain model; the repository does not duplicate domain rules.

## Scope boundaries

M5.3 deliberately does not implement:

- file or database persistence
- lifecycle transitions
- workflow services
- query service abstractions beyond the repository contract
- runtime composition

Those concerns remain subsequent M5 milestones.
