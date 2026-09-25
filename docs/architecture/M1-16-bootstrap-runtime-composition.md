# M1.16 — Bootstrap & Runtime Composition

## Purpose

M1.16 establishes the composition root for the Knowledge runtime.

The bootstrap module now owns the concrete wiring of:

```text
KnowledgeRuntime
      ↓
PersistentKnowledgeService
      ↓
FileKnowledgeRepository
      ↓
FileKnowledgePersistence
      ↓
Filesystem
```

## Responsibility

`KnowledgeRuntime` is responsible for assembling the concrete Knowledge
components required for a file-backed runtime.

The rest of the application does not need to construct persistence and
repository objects manually.

## API

The runtime exposes one composition factory:

`KnowledgeRuntime.fileBacked(Path storageDirectory)`

The resulting runtime exposes the configured:

`PersistentKnowledgeService knowledgeService()`

## Architectural boundary

Concrete infrastructure types remain inside the bootstrap composition path.
The Knowledge service continues to depend on `KnowledgeRepository`, and the
repository continues to depend on `KnowledgePersistence`.

Bootstrap therefore acts as the composition root rather than becoming part
of the Knowledge domain.

## Verification

The tests verify:

- a file-backed runtime can create and retrieve a Knowledge Object
- a second runtime instance can recover previously persisted data

## Non-goals

M1.16 does not introduce:

- Spring Boot
- dependency injection frameworks
- HTTP server startup
- database infrastructure
- application configuration framework
- CLI
- REST endpoints
- cloud deployment

## Definition of Done

- A concrete Knowledge runtime composition exists.
- Infrastructure wiring is centralized in `bootstrap`.
- File-backed persistence works through the runtime entry point.
- Persistence survives runtime recreation.
- Maven reactor remains green.
