# M1.1 -- Platform Foundation Architecture

**Project:** DN Enterprise Consulting -- Enterprise Consulting Platform\
**Milestone:** M1 -- Platform Foundation\
**Work Package:** M1.1 -- Module & Dependency Architecture\
**Status:** Proposed architecture baseline\
**Version:** 0.1.0

## 1. Purpose

M1.1 defines the responsibilities and dependency boundaries of the
existing Maven modules.

The objective is to create a stable foundation for later implementation
of the Knowledge Base, Rules Engine, Assessment Engine and ECP-001
without introducing cyclic dependencies or premature framework coupling.

## 2. Existing Maven Modules

The current `enterprise-platform` root project contains these modules:

-   `platform-bom`
-   `shared-kernel`
-   `assessment`
-   `knowledge`
-   `rules`
-   `publishing`
-   `workflow`
-   `identity`
-   `search`
-   `web-api`
-   `bootstrap`

M0 established that the complete Maven reactor builds successfully.

## 3. Architectural Principles

### 3.1 Dependency direction

Dependencies must point toward stable, lower-level contracts.

Higher-level orchestration may depend on domain/application
capabilities, but lower-level modules must not depend on application
orchestration.

### 3.2 No cyclic module dependencies

The Maven module graph must remain acyclic.

### 3.3 Stable core

`shared-kernel` must remain small. It contains only genuinely
cross-cutting concepts that are stable and independent of individual
product capabilities.

### 3.4 Technology isolation

Business/domain concepts should not require Spring, web frameworks,
persistence frameworks or infrastructure implementations unless there is
a documented architectural reason.

### 3.5 Explicit boundaries

Cross-module communication must use explicit interfaces, domain objects
or contracts. Internal implementation details must not become accidental
public APIs.

## 4. Proposed Module Responsibilities

### `platform-bom`

Central dependency and version management.

Responsibilities: - dependency versions - plugin versions - compatible
platform versions

Restrictions: - no application code - no business logic

### `shared-kernel`

Minimal shared foundation.

Potential contents: - identifiers - common result/error abstractions -
stable shared value types - cross-cutting primitives

Restrictions: - must not contain assessment-specific, knowledge-specific
or publishing-specific concepts.

### `knowledge`

Knowledge access and knowledge-domain contracts.

Responsibilities: - Knowledge Object contracts - knowledge retrieval
abstractions - knowledge validation contracts - interfaces for consuming
versioned knowledge

The authoritative Knowledge Objects themselves belong to
`enterprise-knowledge-base`, not this Maven module.

### `rules`

Rule evaluation contracts and rule execution foundation.

Responsibilities: - rule model - rule evaluation contracts - evaluation
results - rule execution abstractions

It may consume knowledge contracts where required, but must not depend
on the assessment orchestration layer.

### `assessment`

Assessment domain/application capability.

Responsibilities: - assessment model - assessment execution - findings -
assessment context - orchestration of knowledge and rules for an
assessment

`assessment` is a higher-level capability and may depend on `knowledge`
and `rules`.

### `publishing`

Transformation of internal results into defined deliverable
representations.

Responsibilities: - report models - publishing contracts -
rendering/publishing abstractions

It must not own assessment business rules.

### `workflow`

Process orchestration.

Responsibilities: - lifecycle/process coordination - execution state -
orchestration contracts

Workflow should coordinate capabilities rather than duplicate their
domain logic.

### `identity`

Identity and access contracts.

Responsibilities: - principal/identity abstractions - authorization
contracts - security boundary abstractions

Application modules should depend only on the contracts they require.

### `search`

Search/retrieval capability.

Responsibilities: - search contracts - query/result abstractions -
retrieval integration boundary

Search implementation details must remain behind explicit interfaces.

### `web-api`

External API adapter.

Responsibilities: - HTTP/API contracts - request/response mapping - API
error mapping - API-level validation

The web layer must not contain core assessment logic.

### `bootstrap`

Application composition and runtime entry point.

Responsibilities: - dependency wiring - runtime configuration -
application startup - composition of platform components

`bootstrap` is the outermost application module.

## 5. Proposed Dependency Matrix

Allowed dependencies:

  -----------------------------------------------------------------------
  Module                              May depend on
  ----------------------------------- -----------------------------------
  `platform-bom`                      none

  `shared-kernel`                     `platform-bom`

  `identity`                          `shared-kernel`

  `knowledge`                         `shared-kernel`

  `rules`                             `shared-kernel`, `knowledge`

  `search`                            `shared-kernel`, `knowledge`

  `assessment`                        `shared-kernel`, `knowledge`,
                                      `rules`, `identity`

  `publishing`                        `shared-kernel`, `assessment`

  `workflow`                          `shared-kernel`, `assessment`,
                                      `publishing`, `identity`

  `web-api`                           `shared-kernel`, `assessment`,
                                      `workflow`, `identity`, `search`

  `bootstrap`                         platform modules required for
                                      runtime composition
  -----------------------------------------------------------------------

The matrix is an architectural baseline. Individual dependencies should
only be added when actual implementation requires them.

## 6. Forbidden Dependency Patterns

The following are prohibited unless a future architecture decision
explicitly changes the rule:

-   `shared-kernel` → `assessment`
-   `shared-kernel` → `knowledge`
-   `shared-kernel` → `rules`
-   `knowledge` → `assessment`
-   `rules` → `assessment`
-   `assessment` → `web-api`
-   `assessment` → `publishing`
-   `assessment` → `bootstrap`
-   `web-api` → implementation details of lower-level modules
-   any circular dependency

The API layer must adapt the platform; the platform must not adapt
itself to the API.

## 7. Logical Layering

The modules can be viewed in four logical layers:

### Core

``` text
shared-kernel
identity
knowledge
rules
```

### Capability

``` text
assessment
search
publishing
```

### Orchestration / Delivery

``` text
workflow
web-api
```

### Runtime Composition

``` text
bootstrap
```

`platform-bom` is a build/dependency-management concern rather than a
runtime layer.

## 8. Package Convention

Java packages should follow:

``` text
de.dn.enterprise.platform.<module>
```

Examples:

``` text
de.dn.enterprise.platform.assessment
de.dn.enterprise.platform.knowledge
de.dn.enterprise.platform.rules
```

Within a module, prefer explicit boundaries such as:

``` text
domain/
application/
api/
spi/
```

Only introduce `infrastructure/` when a module actually contains
infrastructure-specific implementation.

## 9. API and SPI Rule

Interfaces intended for consumption by another module must be
deliberate.

Use:

-   `api` for contracts exposed to callers
-   `spi` for implementation extension points
-   internal packages for implementation details

Do not expose implementation classes merely because Maven makes them
technically accessible.

## 10. Error Model Baseline

M1 should establish a common error model without prematurely designing
the complete API error contract.

At minimum distinguish:

-   validation failure
-   domain/business rule failure
-   resource not found
-   authorization failure
-   infrastructure/integration failure

Transport-specific mapping belongs to `web-api`.

## 11. Configuration Baseline

Configuration should be:

-   externalizable
-   environment-independent
-   typed where practical
-   free of secrets in source control

Secrets must never be committed to Git.

Runtime configuration belongs to the application/runtime boundary, not
the domain modules.

## 12. Logging and Observability Baseline

M1 establishes only the foundation:

-   structured logging capability
-   correlation/request identifier concept
-   consistent log levels
-   no sensitive data in logs

Detailed observability is deferred until runtime/application
requirements are established.

## 13. Testing Baseline

Every implemented capability must have automated tests.

Baseline:

-   unit tests for domain logic
-   contract tests where module boundaries require them
-   integration tests only where integration behavior is being verified

M1 does not require a full end-to-end test system.

## 14. Dependency Management Rule

All shared third-party versions should be controlled centrally.

`platform-bom` is the preferred location for dependency/version
management.

Individual modules should avoid independently declaring conflicting
versions.

No framework should be introduced merely because it is common. Each
dependency should have a documented technical purpose.

## 15. M1.1 Definition of Done

M1.1 is complete when:

-   module responsibilities are documented
-   dependency direction is documented
-   forbidden dependency patterns are documented
-   package convention is documented
-   API/SPI boundary rule is documented
-   basic error model is documented
-   configuration baseline is documented
-   testing baseline is documented
-   dependency management rule is documented
-   the architecture document is committed to `enterprise-documentation`
-   Maven build remains green after any POM changes

## 16. Next Implementation Step

After M1.1, the next implementation task is:

**M1.2 -- Platform Dependency Baseline**

This should establish only the dependencies and build configuration that
are justified by the foundation.

The next technical checks should remain:

``` bash
mvn -B verify
```

No ECP-001 business logic should be implemented before M1.1 and M1.2 are
stable.
