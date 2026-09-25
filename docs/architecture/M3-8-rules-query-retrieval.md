# M3.8 – Rules Query & Retrieval

## Purpose

M3.8 introduces an explicit query object and query service for the Rules
domain.

The query layer separates retrieval intent from repository infrastructure
and mirrors the query boundary already established in the platform's other
domain modules.

## Query Model

`RuleQuery` supports:

- all rules
- filtering by `RuleType`
- filtering by `RuleStatus`
- combined type and status filtering

Optional filters are combined using AND semantics.

## Query Service

`RuleQueryService` depends only on `RuleRepository`.

The service:

- accepts a `RuleQuery`
- retrieves through the repository abstraction
- applies the requested filters
- returns the matching rules

No file-system, database, REST or search-engine dependency is introduced.

## Architecture

```text
RuleQuery
    |
    v
RuleQueryService
    |
    v
RuleRepository
```

This keeps retrieval intent independent from the concrete persistence
implementation.

## Scope

M3.8 does not introduce:

- full-text search
- ranking
- pagination
- REST endpoints
- database queries
- search-engine integration
- caching

Those concerns remain outside this query/retrieval foundation.
