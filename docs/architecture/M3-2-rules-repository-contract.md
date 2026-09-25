# M3.2 – Rules Repository Contract

## Purpose

M3.2 defines the persistence-independent repository boundary for the Rules domain.

## Contract

`de.dn.enterprise.platform.rules.spi.RuleRepository` exposes:

- `save(Rule)`
- `findById(RuleId)`
- `findAll()`
- `findByType(RuleType)`
- `findByStatus(RuleStatus)`
- `existsById(RuleId)`

The contract uses only Rules domain types and Java collection/`Optional` types.

## Architectural Boundary

The Rules domain/application layer may depend on this SPI. Implementations remain outside the contract and may later use in-memory, file-based, database, or other storage mechanisms.

M3.2 intentionally does not introduce:

- database technology
- JPA/JDBC
- Spring
- REST
- filesystem persistence
- rule evaluation
- rule execution
- caching
- search infrastructure

## Contract Test

The contract test uses a local in-memory stub solely to demonstrate that the SPI can be implemented without infrastructure dependencies.

## Definition of Done

- Repository SPI exists in `rules.spi`.
- Contract covers identity lookup, complete retrieval, type/status filtering, save, and existence.
- No infrastructure technology is introduced.
- Contract test passes.
- Full Maven reactor remains green.
