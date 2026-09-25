# M1.3 — Shared Kernel Foundation

## Purpose

Establish the smallest stable, technology-independent foundation shared by platform modules.

## Scope

The first implementation contains:

- `Identifier` — UUID-based domain identifier value object
- `Version` — minimal semantic version value object
- `DomainException` — base domain exception
- `ValidationException` — validation boundary exception
- `TimeProvider` — injectable UTC time abstraction

## Design rules

1. No Spring dependency.
2. No persistence dependency.
3. No REST/web dependency.
4. No assessment, knowledge, rules, workflow, or publishing concepts.
5. Java SE only in production code.
6. Public types must have a narrow, stable responsibility.
7. Domain invariants are checked at construction/boundary points.
8. Time-dependent code must use an injectable clock rather than calling system time directly.

## Package convention

`de.dn.enterprise.platform.sharedkernel.*`

## Definition of Done

- Production implementation exists in `shared-kernel`.
- Unit tests cover identifier and version behavior.
- The module has no framework/runtime dependency.
- Maven reactor remains green.
- This document is committed under `enterprise-documentation/docs/architecture/`.

## Non-goals

M1.3 does not define entities, aggregates, repositories, event buses, DTOs, API contracts, persistence models, or framework adapters.
