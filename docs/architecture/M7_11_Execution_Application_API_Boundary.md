# M7.11 – Execution Application/API Boundary

## Ziel

M7.11 führt eine stabile Application-Grenze für die Execution-Schicht ein.
Die Application-Grenze kapselt die konkrete Runtime-Komposition und stellt die
fachliche Ausführung eines einzelnen Orchestration Steps bereit.

## Application API

`ExecutionApplication` stellt bereit:

- `execute(OrchestrationStep, ExecutionContext)` – führt einen Step über den zentralen Dispatcher aus.
- `domainKeyFor(OrchestrationStepType)` – löst die Zuordnung eines Step-Typs zum Execution-Domain-Key auf.

## Runtime Integration

`ExecutionRuntime.application()` liefert die stabile Application-Grenze.
Die bestehende Runtime-, Registry- und Dispatcher-Komposition bleibt unverändert
weiterhin verfügbar für die Runtime-Schicht, während externe Use Cases die
Application-Grenze verwenden können.

## Designentscheidung

Es wird keine neue Business-Logik eingeführt. Die Application-Grenze delegiert
an den bestehenden `ExecutionStepDispatcher` und nutzt damit unverändert die
M7.8/M7.9 Registry- und Dispatch-Mechanik.

## Testabdeckung

1. Assessment-Ausführung über die Application-Grenze.
2. Domain-Key-Auflösung für Knowledge, Rules und Publishing.
3. Bestehende M7.10 Runtime-Komposition bleibt funktionsfähig.
