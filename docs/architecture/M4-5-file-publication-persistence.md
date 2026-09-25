# M4.5 File-Based Publication Persistence

## Correction

The initial implementation used a multi-catch block containing `IOException`
and `RuntimeException` and then attempted to rethrow the caught exception.
Java correctly rejects that because the combined catch variable may still be
an `IOException`.

The corrected implementation handles `IOException` separately and keeps
runtime validation/parsing errors inside the deserialization boundary.

## Storage Model

Each publication is stored in one UTF-8 text file:

`<publication-id>.publication`

The persisted representation contains:

1. Base64-encoded publication UUID
2. publication type
3. publication status
4. Base64-encoded metadata name
5. Base64-encoded metadata description
6. zero or more Base64-encoded attribute key/value pairs

Metadata attributes are written in deterministic key order.

## Operations

- `save`
- `load`
- `exists`
- `delete`

The storage directory is created automatically when saving.

Missing loads return `Optional.empty()`. Deleting a missing publication is a
no-op.

## Error Handling

I/O errors and malformed persistence data are wrapped in
`PublicationPersistenceException`.

## Tests

The tests cover round-trip persistence, directory creation, missing objects,
deletion, metadata/status preservation, null arguments, UTF-8/Base64 encoding,
and malformed files.

## Non-Goals

No database, JPA/JDBC, Spring, external serializer, transaction, migration,
search, cache, REST or rendering capability is introduced.
