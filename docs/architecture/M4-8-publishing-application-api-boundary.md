# M4.8 Publishing Application/API Boundary

## Purpose

M4.8 introduces a stable application-facing facade for Publishing.

The application boundary exposes Publishing use cases without exposing
repository or file-persistence infrastructure to callers.

## Architecture

```text
PublicationApplication
        |
        v
PersistentPublicationService
        |
        v
PublicationRepository
        |
        v
FilePublicationRepository
        |
        v
FilePublicationPersistence
        |
        v
*.publication
```

`PublicationRuntime.application()` is the preferred application entry point.

The existing `publicationService()` accessor remains available for
compatibility and lower-level composition/tests.

## Exposed Use Cases

- create
- get
- findAll
- findByType
- findByStatus
- updateMetadata
- publish
- archive

The application facade deliberately contains no persistence implementation
and no file-system code.

## Tests

The API boundary tests verify:

- create/get
- query by type/status
- metadata update
- publish/archive lifecycle
- application construction independent of concrete infrastructure
- runtime application entry point
- persistence across runtime recreation

## Non-Goals

M4.8 does not introduce:

- REST controllers
- HTTP DTOs
- serialization formats for external APIs
- document rendering
- templates
- databases
- authentication/authorization
