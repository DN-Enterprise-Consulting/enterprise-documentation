# M2.1 – Assessment Domain Foundation

## Purpose

M2.1 establishes the technology-neutral domain foundation for assessments.

The first target is the domain model required by the future ECP-001
Enterprise Architecture Assessment product.

## Domain objects

### AssessmentId

Stable UUID-based identifier for an assessment.

### AssessmentType

Initial supported assessment categories:

- ENTERPRISE_ARCHITECTURE
- SOFTWARE_ARCHITECTURE
- APPLICATION_PORTFOLIO
- TECHNOLOGY_LANDSCAPE

### AssessmentStatus

Initial lifecycle states:

- DRAFT
- IN_PROGRESS
- COMPLETED
- ARCHIVED

M2.1 defines the states only. Lifecycle transition rules are deliberately
deferred to a later milestone.

### AssessmentMetadata

Immutable descriptive metadata containing:

- name
- description
- arbitrary string attributes

The name is mandatory. A missing description becomes an empty string and
attributes default to an empty immutable map.

### Assessment

The core assessment aggregate contains:

- id
- type
- status
- metadata

A new assessment is created in DRAFT state.

## Architectural principles

- Java 21
- framework-free domain model
- immutable records where appropriate
- no persistence dependency
- no REST dependency
- no Spring dependency
- no UI concerns
- no assessment lifecycle service yet

## Relationship to M1

M2.1 reuses the shared-kernel `Identifier` abstraction but otherwise keeps
the assessment domain independent from the knowledge persistence
implementation.

## Non-goals

This milestone does not implement:

- assessment questions
- criteria or scoring
- evidence
- findings
- recommendations
- assessment lifecycle services
- repositories
- persistence
- REST APIs
- reporting
- workflow orchestration

## Definition of Done

- assessment domain types exist;
- domain invariants are covered by unit tests;
- no infrastructure framework is introduced;
- architecture documentation is included;
- full Maven verification remains the acceptance criterion.
