# M7.7 – Publishing Step Executor

## Purpose

M7.7 adds the Execution adapter for the Publishing domain.

## Boundary

`PublishingExecutionAdapter` depends on the stable `PublicationApplication` boundary. It does not access the repository or persistence layer directly.

## Execution contract

Input context keys:

- `publication.type` – `PublicationType` enum name
- `publication.name` – required publication name
- `publication.description` – accepted as execution context metadata; the current Publication domain has no description-aware application creation contract, so it is not persisted by this adapter

Execution flow:

1. Read and validate the required type and name.
2. Create a Publication in `DRAFT` through `PublicationApplication`.
3. Publish it through the same application boundary.
4. Return an `ExecutionResult` containing publication ID, type, status and name.

## Current scope

The Publishing domain currently exposes publication lifecycle management, but no renderer/document-content contract. M7.7 therefore establishes lifecycle execution only. Rendering and deliverable generation remain a later concern and must be introduced through an explicit contract rather than inferred here.

## Tests

The adapter tests cover successful creation/publishing, domain-key exposure and missing required context.
