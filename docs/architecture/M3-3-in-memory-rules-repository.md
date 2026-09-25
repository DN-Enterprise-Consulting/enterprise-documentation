# M3.3 – In-Memory Rules Repository

## Purpose

M3.3 provides the first concrete implementation of the Rules repository SPI.
It is intentionally infrastructure-light and keeps persistence concerns out of the domain model.

## Implementation

`de.dn.enterprise.platform.rules.inmemory.InMemoryRuleRepository`
implements `RuleRepository` using a thread-safe `ConcurrentHashMap` keyed by `RuleId`.

Supported operations:

- save
- findById
- findAll
- findByType
- findByStatus
- existsById

Saving an existing `RuleId` replaces the stored rule. Query results are returned as immutable lists.
Null arguments are rejected explicitly.

## Architectural Boundary

The implementation depends on the Rules domain and `rules.spi.RuleRepository` only.
No database, filesystem, Spring, REST, search engine, cache or external serialization technology is introduced.

## Non-goals

- durable persistence
- transactions
- rule evaluation or execution
- query ranking
- pagination
- distributed storage

## Definition of Done

- In-memory repository implements the complete Rules repository contract.
- Unit tests cover save/find, filtering, replacement and invalid arguments.
- No infrastructure technology is introduced.
- Full Maven reactor remains green.
