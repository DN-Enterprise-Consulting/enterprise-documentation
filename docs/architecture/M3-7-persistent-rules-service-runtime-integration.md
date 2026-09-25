# M3.7 – Persistent Rules Service / Runtime Integration

## Purpose

M3.7 adds the application/service boundary above the persistent Rules
repository.

The service operates exclusively against `RuleRepository`; file storage
remains an infrastructure concern.

## Service Responsibilities

`PersistentRuleService` provides:

- rule creation in `DRAFT`
- retrieval by ID
- retrieval of all rules
- retrieval by type
- retrieval by status
- definition updates
- metadata updates
- status changes to `ACTIVE`, `DEPRECATED`, and `ARCHIVED`

`RuleNotFoundException` represents an unknown rule ID.

## Dependency Direction

```text
PersistentRuleService
        |
        v
  RuleRepository
        |
        v
 FileRuleRepository
        |
        v
 RulePersistence
        |
        v
FileRulePersistence
```

The service does not depend on file APIs or persistence implementation
classes.

## Persistence / Restart Semantics

A new `PersistentRuleService` can be created against the same persistent
repository and reload previously stored rules. Integration tests verify
creation, updates, lifecycle changes, queries and restart/reload behavior.

## Scope Boundary

M3.7 does not introduce REST endpoints, transactions, database storage,
search ranking, authorization or workflow orchestration.

Those concerns remain outside the Rules service boundary.
