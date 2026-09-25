# M3.5 – File-Based Rules Persistence

## Purpose

M3.5 provides the first concrete implementation of the `RulePersistence`
contract introduced in M3.4.

The implementation persists one complete `Rule` aggregate per file and uses
only Java SE file APIs.

## File Format

Each `.rule` file is UTF-8 text with:

1. Base64 encoded Rule UUID
2. Rule type
3. Rule status
4. Base64 encoded metadata name
5. Base64 encoded metadata description
6. Base64 encoded rule definition
7. zero or more metadata attributes

Attribute entries use `:` as the separator. Attribute keys and values are
Base64 encoded, so arbitrary UTF-8 values do not conflict with the delimiter.

Metadata attributes are written in sorted key order.

## Behavior

- creates the storage directory automatically
- saves and replaces by Rule ID
- loads the complete Rule state including definition
- checks existence
- deletes by Rule ID
- returns `Optional.empty()` for unknown IDs
- wraps I/O and invalid persisted data in `RulePersistenceException`

## Architectural Constraints

The implementation uses no database, JPA, Hibernate, JDBC, Spring,
external serializer or cloud SDK.

It depends on the Rules domain and the M3.4 persistence SPI.

## Verification

`FileRulePersistenceTest` verifies:

- complete save/load round-trip including rule definition
- unknown Rule lookup
- deletion
- replacement by Rule ID
- null argument handling
