# M4.3 In-Memory Publication Repository

## Correction

The repository `save()` operation stores the publication and returns the
same publication instance. The previous implementation accidentally returned
the value returned by `ConcurrentMap.put()`, which is the previous value and
therefore `null` on first insertion.

The status-filter test also uses two publications with different IDs. This
keeps the test aligned with the repository replacement semantics: saving a
new status for the same ID replaces the previous publication.

## Implementation

`InMemoryPublicationRepository` implements `PublicationRepository` using
`ConcurrentHashMap<PublicationId, Publication>`.

Supported operations:

- save
- findById
- findAll
- findByType
- findByStatus
- existsById

No persistence technology is introduced.
