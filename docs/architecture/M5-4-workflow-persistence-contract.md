# M5.4 – Workflow Persistence Contract

## Purpose

M5.4 defines the persistence SPI for workflow state without coupling the workflow module to a concrete storage technology.

## Contract

`WorkflowPersistence` is located in `de.dn.enterprise.platform.workflow.spi` and provides four operations:

- `save(Workflow)` – stores and returns the supplied workflow.
- `load(WorkflowId)` – loads a workflow by identifier and returns `Optional.empty()` when none exists.
- `exists(WorkflowId)` – checks whether workflow state exists.
- `delete(WorkflowId)` – removes workflow state by identifier.

## Architectural Boundary

The SPI contains only the domain model and Java standard library types. No filesystem, database, serialization, framework, or vendor-specific technology is part of the contract.

Concrete persistence implementations are introduced in later milestones. The contract test uses an in-memory stub solely to prove that the SPI can be implemented independently of persistence technology.

## Contract Guarantees

1. `save` returns the same workflow instance supplied by the caller.
2. A successfully saved workflow can be loaded by its `WorkflowId`.
3. `exists` reports the presence of persisted workflow state.
4. `delete` removes the workflow state.
5. Loading after deletion returns an empty `Optional`.

## Scope

M5.4 intentionally does not define:

- file formats
- database schemas
- transactions
- locking semantics
- serialization formats
- repository integration
- lifecycle rules
- query capabilities
