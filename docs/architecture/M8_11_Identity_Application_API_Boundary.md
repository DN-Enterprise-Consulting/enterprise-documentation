# M8.11 – Identity Application/API Boundary

## Ziel

M8.11 stellt eine stabile Application Boundary für Identity bereit. Aufrufer
müssen weder Repository- noch Persistenzimplementierungen direkt kennen.

## Application API

`IdentityApplication` bündelt:

- create / get
- findAll / find
- findByType / findByStatus / findByTypeAndStatus
- updateMetadata
- activate / lock / disable

Die Application Boundary enthält keine eigene Domänenlogik. Sie delegiert an
die bestehenden `PersistentIdentityService`- und `IdentityQueryService`-
Grenzen.

## Runtime Integration

`IdentityRuntime.application()` liefert die vollständig komponierte
Application Boundary. Die bestehende File-backed Composition bleibt erhalten.

```text
IdentityRuntime
    |
    +-- FileIdentityPersistence
    +-- FileIdentityRepository
    +-- IdentityValidator
    +-- PersistentIdentityService
    +-- IdentityQueryService
    `-- IdentityApplication
```

## Nicht Bestandteil

- Authentication
- Credentials / Passwords
- Tokens / Sessions
- Rollen- und Berechtigungsmodelle
- externe IAM-Integration

Diese Themen bleiben expliziten späteren Boundaries vorbehalten.
