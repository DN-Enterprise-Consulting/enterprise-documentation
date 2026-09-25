# M1.9 – Knowledge Validation & Invariants

## Purpose

M1.9 introduces explicit validation at the application boundary for Knowledge Objects.

The objective is to prevent structurally invalid Knowledge Objects from being persisted through `KnowledgeService`.

## Validation rules

The following invariants are enforced:

1. The Knowledge Object itself must not be null.
2. Metadata must not be null.
3. `metadata.name` must not be blank.
4. `content` must not be null or blank.

The existing domain constructors continue to enforce their own nullability and structural constraints. M1.9 adds service-level validation before repository writes.

## Validation flow

```text
create / update
      |
      v
KnowledgeObject
      |
      v
KnowledgeObjectValidator
      |
   valid?
   /    \
 yes     no
  |       |
  v       v
repository  KnowledgeValidationException
```

## Transactional safety

Validation happens before `repository.save(...)`.

Therefore an invalid create does not add an object to the repository, and an invalid update does not replace the existing valid object.

## Design decisions

- Validation is framework-free.
- Validation is explicit and deterministic.
- Validation is located outside the repository implementation.
- Repository implementations are not responsible for application-level validation.
- Lifecycle validation remains in `KnowledgeLifecycleService`.

## Non-goals

M1.9 does not introduce:

- Bean Validation / Jakarta Validation
- REST validation
- persistence constraints
- authorization
- business-rule engines
- cross-object validation
- content schema validation

## Definition of Done

- `KnowledgeObjectValidator` implemented.
- `KnowledgeValidationException` implemented.
- Create validates before persistence.
- Update validates before persistence.
- Invalid objects cannot overwrite valid repository state.
- Unit and service-level tests cover the invariants.
- No infrastructure framework dependency introduced.
