# M2.3 – Assessment Repository & Persistence Contract

## Purpose

M2.3 establishes the repository and persistence boundaries for the Assessment domain without introducing a concrete storage technology.

## AssessmentRepository

`AssessmentRepository` is the domain-facing repository SPI.

Supported operations:

- `save`
- `findById`
- `findAll`
- `findByType`
- `findByStatus`
- `existsById`

The repository exposes domain objects and domain identifiers. It does not expose database, filesystem, ORM or transport details.

## AssessmentPersistence

`AssessmentPersistence` is the lower-level persistence SPI.

Supported operations:

- `save`
- `load`
- `loadAll`
- `exists`
- `delete`

The contract deliberately does not prescribe a persistence technology.

## Boundary

```text
Assessment Service / Domain
          |
          v
AssessmentRepository
          |
          v
AssessmentPersistence
          |
          v
future concrete persistence implementation
```

The concrete persistence implementation is explicitly outside M2.3.

## Design rules

- no Spring dependency
- no JPA/Hibernate dependency
- no JDBC dependency
- no SQL
- no database driver
- no filesystem implementation
- no REST dependency
- no search engine dependency
- no infrastructure-specific types in the SPI

## Compatibility with M2.1 / M2.2

The Assessment domain and lifecycle remain unchanged.

M2.3 only introduces the persistence boundaries required for the next implementation step.

## Definition of Done

- repository SPI defined
- persistence SPI defined
- repository contract test added
- persistence contract test added
- no concrete persistence technology introduced
- architecture documentation added
- full Maven reactor remains green
