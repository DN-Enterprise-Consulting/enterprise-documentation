# M2.8 – Assessment Validation

## Ziel

M2.8 ergänzt eine explizite Validierungsschicht für das Assessment-Modell.

Die Validierung stellt sicher, dass Assessments vor einer Verarbeitung durch Application-/Service-Schichten die erwarteten strukturellen Voraussetzungen erfüllen.

## Validierungsregeln

`AssessmentValidator` prüft:

- Assessment ist nicht `null`
- `type` ist gesetzt
- `status` ist gesetzt
- `metadata` ist gesetzt
- `metadata.name` ist nicht `null` und nicht blank

Bei einer Verletzung wird `AssessmentValidationException` ausgelöst.

## Architektur

```text
Assessment
    |
    v
AssessmentValidator
    |
    v
AssessmentService
    |
    v
AssessmentRepository
```

Die Validierung ist frameworkfrei und kennt keine Persistence-Technologie.

## Abgrenzung

Die bestehende Domain darf weiterhin eigene Invarianten durch Konstruktoren bzw. Value Objects durchsetzen. Die Validierungsschicht ersetzt diese Domain-Invarianten nicht, sondern bildet die explizite Validierungsgrenze für Service-/Application-Operationen.

Nicht Bestandteil von M2.8:

- REST/API-Validierung
- Bean Validation / Jakarta Validation
- Datenbankvalidierung
- Persistence-Schema-Validierung
- fachliche Bewertungsregeln eines Assessments
