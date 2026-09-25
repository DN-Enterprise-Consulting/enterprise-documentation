# M1.17 — Runtime Configuration

## Purpose

M1.17 separates Knowledge runtime configuration from runtime composition.

The runtime is now created from an immutable
`KnowledgeRuntimeConfiguration` instead of receiving a raw `Path` directly.

## Runtime flow

```text
KnowledgeRuntimeConfiguration
              ↓
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

## Configuration

The configuration currently contains one explicit property:

- `storageDirectory`

The configuration is immutable and rejects a null storage directory.

The factory

`KnowledgeRuntimeConfiguration.fileBacked(Path)`

expresses the intended current storage mode without introducing an external
configuration framework.

## Composition

`KnowledgeRuntime.create(configuration)` is the composition entry point.

The runtime remains responsible for constructing the concrete repository and
persistence adapter. The configuration object carries the infrastructure
settings into that composition root.

## Architectural benefit

The service and repository remain independent of configuration mechanics.

A later milestone can introduce environment variables, application
configuration files or another configuration provider without changing the
Knowledge service contract.

## Non-goals

M1.17 does not introduce:

- Spring configuration
- YAML/properties frameworks
- environment-variable parsing
- database configuration
- secrets management
- dependency injection frameworks
- REST configuration

## Definition of Done

- Runtime configuration is represented by an immutable type.
- Runtime creation consumes the configuration type.
- File-backed persistence still works.
- Persistence survives runtime recreation.
- Null configuration values are rejected.
- Maven reactor remains green.
