# M5.2 – Workflow Repository Contract

## Purpose

M5.2 defines the persistence-independent repository boundary for the Workflow domain.

## Contract

`de.dn.enterprise.platform.workflow.spi.WorkflowRepository` exposes:

- `save(Workflow)`
- `findById(WorkflowId)`
- `findAll()`
- `findByType(WorkflowType)`
- `findByStatus(WorkflowStatus)`
- `existsById(WorkflowId)`

The contract uses only Workflow domain types and Java `List` / `Optional` abstractions.

## Architectural Boundary

The Workflow domain/application layer may depend on this SPI. Implementations remain outside the contract and may later use in-memory, file-based, database, or other storage mechanisms.

M5.2 intentionally does not introduce:

- database technology
- JPA/JDBC
- Spring
- REST
- filesystem persistence
- workflow execution
- orchestration
- caching
- search infrastructure

## Contract Test

The contract test uses a local in-memory stub solely to demonstrate that the SPI can be implemented without infrastructure dependencies.

## Definition of Done

- Repository SPI exists in `workflow.spi`.
- Contract covers identity lookup, complete retrieval, type/status filtering, save, and existence.
- No infrastructure technology is introduced.
- Contract test passes.
- Full Maven reactor remains green.
