# M3.1 – Rules Domain Foundation

## Purpose

M3.1 establishes the framework-free domain model for rules. It provides the stable domain vocabulary required by later rule repository, lifecycle, validation and evaluation milestones.

## Domain model

- `RuleId` – immutable UUID-based identifier.
- `RuleType` – high-level classification of a rule: architecture, compliance, security, quality, technology or custom.
- `RuleStatus` – lifecycle state: `DRAFT`, `ACTIVE`, `DEPRECATED`, `ARCHIVED`.
- `RuleMetadata` – name, description and immutable attributes.
- `Rule` – aggregate root containing identity, type, status, metadata and a textual rule definition.

## Invariants

- All mandatory fields are non-null.
- Rule names are non-blank.
- Rule definitions are non-null and non-blank.
- Metadata attributes are defensively copied.
- Domain objects are immutable records.

## Factory and mutation semantics

`Rule.draft(...)` creates a new rule in `DRAFT` state with a generated identifier.

`withStatus`, `withMetadata` and `withDefinition` return new immutable instances and preserve the rule identifier.

Lifecycle transition rules are intentionally not implemented in M3.1; they belong to the subsequent lifecycle milestone.

## Dependency boundary

The model is framework-free. It uses Java SE only and has no database, REST, Spring, persistence or rule-engine dependency.

The `rules` module may depend on `shared-kernel`, but M3.1 keeps the rule domain model self-contained so later infrastructure can be introduced behind explicit contracts.

## Non-goals

M3.1 does not implement:

- rule persistence
- rule repository
- rule validation service
- rule evaluation engine
- expression parsing
- rule execution
- REST/API exposure
- database integration

## Definition of Done

- Domain classes compile on Java 21.
- Domain objects are immutable.
- Core invariants are enforced.
- Unit tests cover construction and immutable update semantics.
- No framework or infrastructure dependency is introduced.
