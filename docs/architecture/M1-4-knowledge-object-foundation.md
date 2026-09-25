# M1.4 – Knowledge Object Foundation

## Purpose

M1.4 establishes the first domain-level foundation for the platform's Knowledge module.

The implementation is intentionally technology-independent. It does not introduce persistence, REST, Spring, search infrastructure, or workflow concerns.

## Domain model

A Knowledge Object consists of:

- stable identity: `KnowledgeObjectId`
- semantic type: `KnowledgeObjectType`
- version: `KnowledgeObjectVersion`
- lifecycle status: `KnowledgeObjectStatus`
- metadata: `KnowledgeObjectMetadata`
- content: textual content

## Design principles

- Immutable domain objects using Java records.
- Explicit validation at construction boundaries.
- Stable identity is independent from version.
- Version changes create new instances rather than mutating an object.
- Metadata attributes are defensively copied.
- No framework dependencies.

## Initial lifecycle

The foundation defines these states:

`DRAFT`, `ACTIVE`, `DEPRECATED`, `ARCHIVED`

Lifecycle transitions are deliberately not encoded as a workflow engine in M1.4.

## Versioning

The foundation uses a three-component version:

`major.minor.patch`

The API provides explicit next-major, next-minor and next-patch operations.

## Non-goals

M1.4 does not define:

- persistence schema
- repository interfaces
- search indexing
- authorization
- workflow orchestration
- rendering or publishing
- HTTP/API contracts

These concerns belong to later increments.

## Definition of Done

- Knowledge Object domain model implemented.
- Identity, type, version and lifecycle status represented explicitly.
- Metadata is immutable from the caller's perspective.
- Domain validation covered by unit tests.
- No infrastructure dependencies introduced.
- Maven reactor remains green.
