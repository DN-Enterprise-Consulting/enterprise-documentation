# M5.6 – Persistent Workflow Repository

## Ziel

M5.6 verbindet das bestehende `WorkflowRepository`-SPI mit der
dateibasierten `WorkflowPersistence`.

## Umsetzung

- `FileWorkflowRepository` implementiert `WorkflowRepository`.
- `save`, `findById` und `existsById` delegieren an `WorkflowPersistence`.
- `findAll`, `findByType` und `findByStatus` arbeiten auf den persistierten
  Workflow-Objekten.
- `FileWorkflowPersistence.loadAll()` liest `.workflow`-Dateien in
  deterministischer Dateinamenreihenfolge.
- Ein neu erzeugtes Repository kann bereits persistierte Workflows wieder
  laden.
- Es wird keine Datenbank und kein Framework eingeführt.

## Architekturgrenze

Die Repository-Implementierung hängt gegen die abstrakten Workflow-SPIs.
Für die Aufzählung aller dateibasierten Datensätze nutzt sie die konkrete
`FileWorkflowPersistence`, da das bestehende `WorkflowPersistence`-SPI
bewusst nur Einzelobjekt-Operationen definiert.

Eine spätere SPI-Erweiterung kann diese Übergangslösung ersetzen, ohne die
fachliche Repository-API zu verändern.

## Definition of Done

- File-backed Repository vorhanden
- Persistenz über Repository-Neustart nachgewiesen
- `findAll`, `findByType`, `findByStatus` implementiert
- Null-Argumente abgesichert
- Unit-/Integration-Tests vorhanden
- keine Datenbankabhängigkeit
- `mvn -B clean verify` erfolgreich
