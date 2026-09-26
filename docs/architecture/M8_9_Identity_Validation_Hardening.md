# M8.9 Identity Validation & Hardening

## Scope

M8.9 strengthens the identity boundary with explicit validation and service-level hardening.

## Implemented

- `IdentityValidator` validates identity aggregates at application and persistence boundaries.
- `IdentityValidationException` represents invalid identity state.
- `PersistentIdentityService` validates identities before persistence and validates repository results after persistence/load.
- Existing lifecycle rules remain unchanged: ACTIVE -> LOCKED / DISABLED; LOCKED -> ACTIVE / DISABLED; DISABLED is terminal.
- Null repository and validator arguments are rejected.
- Tests cover valid identities, supported statuses, null handling and repository boundary hardening.

## Non-goals

Authentication, credentials, passwords, tokens, roles, permissions and external identity providers are outside M8.9.
