# M1.7 – Knowledge Service

## Purpose

M1.7 introduces the first application-level service for Knowledge Objects.

The service coordinates the existing Knowledge domain model and repository SPI without introducing persistence or transport technology.

## Application flow

```text
Caller
  |
  v
KnowledgeService
  |
  +--> KnowledgeObject domain model
  |
  +--> KnowledgeRepository
           |
           +--> InMemoryKnowledgeRepository
```

## Operations

### `create()`

Creates a new Knowledge Object in `DRAFT` status using a generated stable identifier and initial version `1.0.0`.

### `get()`

Retrieves an object by identifier. Missing objects result in `KnowledgeObjectNotFoundException`.

### `findByType()`

Delegates type-based retrieval to the repository.

### `updateContent()`

Creates a new immutable Knowledge Object instance with the same identity, incremented patch version and current lifecycle status.

### `activate()`

Creates a new instance with `ACTIVE` status.

### `deprecate()`

Creates a new instance with `DEPRECATED` status.

## Design rules

- The service depends on the repository SPI, not a concrete storage implementation.
- Domain objects remain immutable.
- Persistence remains outside the service.
- No Spring, REST or database dependency is introduced.
- Missing-object behavior is explicit through a dedicated exception.

## Lifecycle scope

M1.7 intentionally does not define a complete lifecycle state machine. It provides the initial application operations needed by the platform foundation.

## Non-goals

M1.7 does not define:

- REST controllers
- transactions
- authorization
- database persistence
- search
- workflow orchestration
- event publication

## Definition of Done

- KnowledgeService implemented.
- Create, get, find-by-type, update, activate and deprecate operations covered.
- Missing-object behavior covered.
- Repository dependency is expressed through the M1.5 SPI.
- No infrastructure framework dependency introduced.
- Maven reactor remains green.
