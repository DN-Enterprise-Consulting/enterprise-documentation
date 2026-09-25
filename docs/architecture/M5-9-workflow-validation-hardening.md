# M5.9 – Workflow Validation & Hardening

## Scope

M5.9 adds explicit workflow validation and hardening around the persistent workflow service.

## Validation

`WorkflowValidator` validates workflow identity, type, status and metadata. Metadata validation checks the name, description, attributes map and attribute entries.

## Service integration

`PersistentWorkflowService` validates created, loaded, queried, updated and transitioned workflows. The validator is injected through an overloadable constructor while the default constructor keeps the existing composition simple.

## Hardening

The service rejects null collaborators and required arguments, prevents metadata changes after leaving `DRAFT`, and preserves the terminal nature of `COMPLETED` and `FAILED` through the existing lifecycle rules.

## Architectural boundary

Validation remains framework-free and lives inside the workflow module. Persistence and repository implementations remain behind the existing SPI boundaries.
