# M7.9 – Execution Step Dispatcher

## Ziel

M7.9 verbindet die Cross-Domain-Orchestration-Step-Typen mit der zentralen Execution-Adapter-Registry.

Der Dispatcher entscheidet anhand des `OrchestrationStepType`, welcher stabile Domain-Key verwendet wird, löst den zugehörigen `ExecutionAdapter` aus der Registry auf und delegiert die Ausführung mit dem unveränderten `ExecutionContext`.

## Mapping

| OrchestrationStepType | Domain-Key |
|---|---|
| `ASSESSMENT` | `assessment` |
| `RULE_EVALUATION` | `rules` |
| `KNOWLEDGE_PROCESSING` | `knowledge` |
| `PUBLICATION` | `publishing` |

## Verantwortungsgrenze

M7.9 führt weiterhin keine mehrstufige Orchestration aus. Es dispatcht genau einen `OrchestrationStep`.

Nicht enthalten sind:
- Lifecycle-Steuerung der Orchestration
- Persistierung von Execution-Ergebnissen
- automatische Ausführung mehrerer Steps
- Retry- oder Recovery-Mechanismen
- fachliche Regelbewertung

Diese Verantwortlichkeiten bleiben nachgelagerten Milestones vorbehalten.

## Fehlerverhalten

- `null` für Step oder Context wird abgelehnt.
- Ein unbekannter Step-Typ führt zu `ExecutionStepDispatchException`.
- Für einen gültig gemappten, aber nicht registrierten Adapter wird der bestehende Registry-Fehler weitergereicht.
