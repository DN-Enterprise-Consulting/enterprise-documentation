# M3.9 – Rules Validation & Lifecycle Hardening

## Purpose

M3.9 hardens the Rules domain by making validation and lifecycle rules
explicit and reusable at the service boundary.

## Validation

`RuleValidator` verifies:

- rule exists
- ID, type and status are present
- metadata is present
- metadata name is not blank
- rule definition is not blank

`RuleValidationException` represents validation failures.

The validator does not replace domain constructor invariants. It provides an
explicit validation boundary for application/service flows.

## Lifecycle

The supported lifecycle is:

```text
DRAFT -> ACTIVE -> DEPRECATED -> ARCHIVED
```

Only adjacent forward transitions are allowed.

`ARCHIVED` is terminal.

The same-status transition is idempotent.

`RuleLifecycleService` owns the transition policy and produces a new
immutable Rule instance.

## Service Hardening

Persistent rule operations must not bypass lifecycle rules:

- a DRAFT rule cannot be deprecated or archived directly
- an ARCHIVED rule cannot be modified

## Architecture

```text
PersistentRuleService
       |
       +----> RuleValidator
       |
       +----> RuleLifecycleService
       |
       v
 RuleRepository
```

The hardening layer remains independent of file storage, databases, REST
and search infrastructure.

## Scope

M3.9 does not introduce REST APIs, authorization, rule execution/evaluation,
transactions or external persistence technologies.
