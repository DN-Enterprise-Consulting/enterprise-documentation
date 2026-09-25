# M1.2 Platform Dependency Baseline

## Purpose
M1.2 establishes one central Maven dependency and build baseline for the enterprise-platform repository. Module POMs must not independently choose versions for centrally managed dependencies.

## Baseline
- Java: 21
- Encoding: UTF-8
- Maven project version: 0.1.0-SNAPSHOT
- Dependency versions are managed centrally in `platform-bom`.
- Build plugins are managed by the root build where possible.
- Application/runtime frameworks are introduced only by modules that need them; they are not placed into `shared-kernel` by default.

## Dependency management policy
1. A dependency version must be declared once in `platform-bom`.
2. Child modules consume managed dependencies without repeating versions.
3. `shared-kernel` remains framework-light.
4. Test dependencies are centrally managed.
5. Platform modules may add implementation-specific dependencies only when required by their responsibility.
6. No dependency may be introduced solely for convenience into a lower layer.

## Initial centrally managed baseline
The initial baseline deliberately contains only broadly reusable libraries:

- SLF4J API for logging contracts
- JUnit Jupiter for tests
- AssertJ for readable assertions

The baseline does not yet force Spring Boot, persistence, HTTP, Kubernetes or cloud SDK dependencies into every module. Those belong in later capability-specific increments.

## Build quality gates
The root build should enforce:
- Java 21 compilation
- UTF-8 source/resource encoding
- deterministic test execution through Surefire
- dependency convergence / duplicate dependency hygiene as the platform grows

## Module rule
Each module POM should contain only dependencies required by that module and should normally omit `<version>` when the dependency is managed by `platform-bom`.

## Definition of Done
- `platform-bom` is a valid Maven BOM.
- Root POM imports `platform-bom` under dependencyManagement.
- Core test/logging dependencies are centrally versioned.
- No module contains duplicate version declarations for centrally managed dependencies.
- `mvn -B verify` remains green.
- This document is committed under `enterprise-documentation/docs/architecture/`.

## Next step
M1.3: Shared Kernel foundation — introduce only the minimal cross-cutting primitives required by the module contracts.
