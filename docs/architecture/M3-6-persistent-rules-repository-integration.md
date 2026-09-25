# M3.6 – Persistent Rules Repository Integration

## Purpose

M3.6 integrates the M3.4 `RulePersistence` SPI with the M3.2
`RuleRepository` contract.

`FileRuleRepository` provides the repository boundary while
`FileRulePersistence` remains responsible for physical file storage.

## Responsibilities

### FileRuleRepository

- delegates `save` to persistence
- delegates `findById` to persistence
- delegates `existsById` to persistence
- exposes `findAll`
- derives `findByType` and `findByStatus` from persisted rules

### FileRulePersistence

- persists one Rule per `.rule` file
- reconstructs the complete Rule state
- provides `loadAll` for repository-wide retrieval

## Dependency Direction

```text
RuleRepository
      ^
      |
FileRuleRepository
      |
      v
RulePersistence
      ^
      |
FileRulePersistence
```

The repository depends on the persistence SPI rather than on file APIs.

## Transitional Boundary

The M3.4 `RulePersistence` contract currently exposes single-object
operations only. Therefore `FileRuleRepository.findAll()` uses the concrete
`FileRulePersistence` `loadAll()` capability.

This is intentionally documented as a transitional boundary. A future
extension of the persistence SPI with a bulk-load operation can remove the
concrete-type check without changing the repository contract.

## Verification

Integration tests verify:

- repository save and reload after repository recreation
- persistent `findAll`
- persistent filtering by type and status
- repository visibility after deletion
- null argument handling

No database, Spring, JPA, JDBC, external serializer or cloud SDK is used.
