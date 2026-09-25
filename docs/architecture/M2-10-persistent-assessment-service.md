# M2.10 – Persistent Assessment Service & Runtime Integration

## Ziel

M2.10 verbindet die bisher getrennten Assessment-Bausteine zu einer
persistierenden Application-Service-Grenze.

## Komponenten

```text
AssessmentRuntime
      |
      +--> PersistentAssessmentService
      |       +--> AssessmentValidator
      |       +--> AssessmentLifecycleService
      |       +--> AssessmentRepository
      |
      +--> AssessmentQueryService
      |
      +--> AssessmentValidator
      |
      +--> FileAssessmentRepository
              |
              +--> FileAssessmentPersistence
```

## Verantwortlichkeiten

`PersistentAssessmentService` ist die Application-Service-Grenze für
persistente Assessment-Operationen:

- create
- get
- updateMetadata
- start
- complete
- archive

Vor jeder Persistierung wird das Assessment validiert. Lifecycle-Übergänge
werden ausschließlich über `AssessmentLifecycleService` ausgeführt.

## Persistenz

Der Service kennt nur `AssessmentRepository`. Die konkrete Dateipersistenz
bleibt an der Runtime-Composition-Grenze.

Damit kann die gleiche Application-Service-Logik später mit einer anderen
Repository-Implementierung betrieben werden.

## Runtime-Integration

`AssessmentRuntime` stellt den `PersistentAssessmentService` zusätzlich zu
Repository, Query Service und Validator bereit.

Ein End-to-End-Test prüft, dass ein Assessment nach Runtime-Neuaufbau aus der
Dateipersistenz wieder geladen und über den Query Service gefunden werden kann.

## Architekturregeln

- Domain kennt keine Persistenz.
- Application Service kennt nur Repository/SPI und Fachlogik.
- Lifecycle bleibt zentralisiert.
- Validation ist Teil des Persistierungswegs.
- Runtime ist die Composition Boundary.
- Keine Spring-, JPA-, JDBC-, REST- oder Datenbankabhängigkeiten.
