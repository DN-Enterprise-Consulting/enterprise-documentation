# M4.1 – Publishing Domain Foundation

## Purpose

M4.1 establishes the framework-free domain foundation for the Publishing module.

The domain models the publication artifact produced by later publishing and rendering workflows. It intentionally contains no persistence, REST, Spring, rendering engine, filesystem, or document-format implementation.

## Domain model

### PublicationId

UUID-based immutable identifier.

### PublicationType

Initial publication categories:

- `ASSESSMENT_REPORT`
- `ARCHITECTURE_DOCUMENT`
- `EXECUTIVE_SUMMARY`
- `TECHNICAL_REPORT`

### PublicationStatus

Initial lifecycle states:

- `DRAFT`
- `PUBLISHED`
- `ARCHIVED`

Lifecycle transition rules are deliberately not implemented in M4.1. They belong to a later lifecycle/application step.

### PublicationMetadata

Immutable publication metadata:

- required name
- optional description
- immutable attributes map

### Publication

Immutable aggregate-like domain record containing:

- `PublicationId`
- `PublicationType`
- `PublicationStatus`
- `PublicationMetadata`

A new publication is created in `DRAFT`.

## Design principles

- Java 21
- framework-free domain model
- immutable records
- explicit null/invariant checks
- no persistence technology
- no REST/API adapter
- no rendering implementation
- no document generation library
- no dependency on assessment, knowledge, or rules

## M4.1 non-goals

M4.1 does not define:

- publication repository
- publication persistence
- rendering
- templates
- PDF/Word/HTML generation
- report composition
- REST endpoints
- workflow orchestration
- lifecycle enforcement

These concerns will be introduced incrementally in subsequent M4 steps.
