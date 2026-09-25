# M1.8 – Knowledge Lifecycle Rules

## Purpose

M1.8 defines and enforces the lifecycle of a Knowledge Object.

## State model

```text
DRAFT -> ACTIVE -> DEPRECATED -> ARCHIVED
```

Only the transitions shown above are allowed.

## Transition rules

| Current | Operation | Target |
|---|---|---|
| DRAFT | activate | ACTIVE |
| ACTIVE | deprecate | DEPRECATED |
| DEPRECATED | archive | ARCHIVED |

All other lifecycle transitions are rejected.

`ARCHIVED` is a terminal state.

## Update rule

Knowledge Objects may be updated while they are not archived. An update preserves the lifecycle status and increments the patch version.

Example:

```text
ACTIVE 1.0.0
   |
   | update
   v
ACTIVE 1.0.1
```

An archived object cannot be updated.

## Separation of concerns

Lifecycle status and version are independent concepts:

```text
Lifecycle:
DRAFT -> ACTIVE -> DEPRECATED -> ARCHIVED

Version:
1.0.0 -> 1.0.1 -> 1.0.2
```

## Implementation

`KnowledgeLifecycleService` contains the lifecycle transition rules.

`KnowledgeService` remains the application-level facade and delegates lifecycle decisions to the lifecycle service.

## Non-goals

M1.8 does not introduce:

- persistence
- REST
- authorization
- audit events
- event sourcing
- workflow orchestration
- database transactions

## Definition of Done

- Lifecycle transitions are explicit.
- Invalid transitions raise `KnowledgeLifecycleException`.
- ARCHIVED is terminal.
- Archived objects cannot be updated.
- Complete lifecycle is covered by tests.
- No infrastructure framework dependency is introduced.
