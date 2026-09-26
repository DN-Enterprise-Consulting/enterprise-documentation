# M8.8 – Identity Query / Retrieval

## Ziel

Einführung einer Query-Schicht oberhalb des `IdentityRepository`.
Die Query-Schicht kapselt die fachliche Filterung nach Identity-Typ und Status.

## Architektur

`IdentityQueryService`
↓
`IdentityRepository`

Die Query-Schicht kennt keine konkrete Persistence-Technologie.

## Query-Modell

`IdentityQuery` unterstützt:

- keine Filter → alle Identities
- `IdentityType`
- `IdentityStatus`
- Kombination aus Type und Status

Kombinationen verwenden AND-Semantik.

## API

- `IdentityQuery.all()`
- `IdentityQuery.byType(type)`
- `IdentityQuery.byStatus(status)`
- `IdentityQuery.byTypeAndStatus(type, status)`
- `IdentityQueryService.find(query)`
- `IdentityQueryService.findAll()`

## Abgrenzung

M8.8 implementiert keine Pagination, Sortierungsparameter,
Volltextsuche oder Security-/Authorization-Filter.


## Test-Hardening

Query-Ergebnisse übernehmen die deterministische Reihenfolge des
`IdentityRepository`. Die Tests vergleichen deshalb ebenfalls nach
Identity-ID sortiert und hängen nicht von der zufälligen UUID-Erzeugungsreihenfolge ab.
