# M2.9 – Assessment Runtime Composition

## Ziel

M2.9 definiert die Composition Boundary für das Assessment-Modul und stellt
eine reproduzierbare, dateibasierte Runtime-Zusammensetzung bereit.

## Komponenten

```text
AssessmentRuntimeConfiguration
            |
            v
AssessmentRuntimeFactory
            |
            v
     AssessmentRuntime
            |
            +--> FileAssessmentPersistence
            |
            +--> FileAssessmentRepository
            |
            +--> AssessmentQueryService
            |
            +--> AssessmentValidator
```

## Verantwortlichkeiten

### AssessmentRuntimeConfiguration

Enthält ausschließlich die für die Runtime benötigte Storage-Konfiguration.
Aktuell ist die dateibasierte Ablage die unterstützte Runtime-Variante.

### AssessmentRuntime

Komponiert die konkreten Infrastrukturbausteine und stellt die zentralen
Assessment-Laufzeitkomponenten bereit:

- AssessmentRepository (`assessment.spi`)
- AssessmentQueryService
- AssessmentValidator

Die Runtime kapselt die konkrete Dateipersistenz.

### AssessmentRuntimeFactory

Stabile Einstiegstelle für die Erstellung einer AssessmentRuntime.

## Architekturregeln

- Composition erfolgt ausschließlich an der Runtime-Grenze.
- Fachliche Komponenten kennen keine Dateisystemdetails.
- Das Repository hängt gegen den Persistence-Vertrag.
- Query Service hängt gegen das Repository-Interface.
- Keine Spring-, JPA-, JDBC- oder REST-Abhängigkeiten.
- Keine direkte Persistence-Implementierung in Domain- oder Query-Code.
- Die konkrete Dateipersistenz bleibt austauschbar.

## Abgrenzung

M2.9 führt noch keinen Web-Adapter und keine Datenbankpersistenz ein.
Service-Orchestrierung und übergeordnete Workflow-Komposition bleiben
nachgelagerten Milestones vorbehalten.
