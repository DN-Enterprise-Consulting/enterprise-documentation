# M2.7 – Assessment Query & Retrieval

## Ziel

M2.7 ergänzt die Assessment-Domäne um eine technologieunabhängige Query- und Retrieval-Schicht.

Unterstützt werden Abruf aller Assessments, Filterung nach `AssessmentType`, Filterung nach `AssessmentStatus` sowie kombinierte Filterung.

## Architektur

```text
AssessmentQuery
       |
       v
AssessmentQueryService
       |
       v
AssessmentRepository
       +--> InMemoryAssessmentRepository
       +--> FileAssessmentRepository
```

`AssessmentQueryService` kennt ausschließlich das Repository-SPI. Konkrete Persistence-Technologien sind nicht Bestandteil der Query-Schicht.

## Query-Modell

`AssessmentQuery` enthält zwei optionale Filter: `AssessmentType` und `AssessmentStatus`. Sind beide gesetzt, werden sie mit logischem AND kombiniert.

## API

```java
AssessmentQuery.all();
AssessmentQuery.byType(type);
AssessmentQuery.byStatus(status);
AssessmentQuery.byTypeAndStatus(type, status);
```

Convenience-Methoden des Service: `findAll()`, `findByType(type)`, `findByStatus(status)`, `findByTypeAndStatus(type, status)`.

## Nicht Bestandteil

Volltextsuche, Ranking, Pagination, Sortierung, REST, externe Search Engine und Caching.
