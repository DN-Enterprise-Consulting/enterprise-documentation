# M3.4 – Rules Persistence Contract

## Purpose

M3.4 defines the technology-neutral persistence boundary for the Rules domain.

The contract allows the Rules application/repository layer to persist and
reload `Rule` aggregates without depending on a concrete storage technology.

## SPI

Package:

`de.dn.enterprise.platform.rules.spi`

Interface:

`RulePersistence`

Operations:

- `save(Rule rule)` – stores or replaces a Rule aggregate and returns the persisted aggregate.
- `load(RuleId id)` – loads a Rule by identifier.
- `exists(RuleId id)` – checks whether a Rule exists.
- `delete(RuleId id)` – removes a Rule by identifier.

## Architectural Boundary

```text
Rules Application / Repository
            |
            v
     RulePersistence
            |
            v
   Concrete persistence
```

The Rules domain does not depend on the concrete persistence implementation.

## Non-Goals

M3.4 does not introduce:

- database technology
- JPA/Hibernate
- JDBC
- SQL schema
- migrations
- transactions
- file persistence
- Spring
- REST/API adapters
- caching
- search infrastructure
- optimistic locking

Those concerns belong to later implementation milestones.

## Compatibility

The SPI references only the existing Rules domain types:

- `Rule`
- `RuleId`

No new Rule domain concepts are introduced by M3.4.

## Verification

`RulePersistenceContractTest` verifies the public method signatures of the
persistence boundary without coupling the contract test to a concrete
persistence implementation.

## Definition of Done

- `RulePersistence` exists in the Rules SPI package.
- CRUD-oriented persistence operations are explicitly defined.
- No concrete persistence technology is introduced.
- Contract test verifies the public API shape.
- Architecture documentation records the boundary and non-goals.
