# M7.10 – Execution Runtime Composition

## Ziel

M7.10 verbindet die in M7 aufgebauten Execution-Bausteine zu einer ausführbaren Runtime-Komposition.

## Zusammensetzung

```text
ExecutionRuntime
  ├── AssessmentRuntime
  ├── KnowledgeRuntime
  ├── Rule persistence + PersistentRuleService
  ├── PublicationRuntime
  ├── InMemoryExecutionAdapterRegistry
  │     ├── assessment
  │     ├── knowledge
  │     ├── rules
  │     └── publishing
  └── ExecutionStepDispatcher
```

Die Runtime verwendet für jeden Fachbereich einen eigenen Storage-Unterordner:

- `assessment/`
- `knowledge/`
- `rules/`
- `publishing/`

## API

`ExecutionRuntime.fileBacked(Path)` ist die zentrale Composition-Factory.

Die Runtime stellt bereit:

- `registry()` – registrierte Execution Adapter
- `dispatcher()` – zentraler Step-Dispatch
- `assessmentRuntime()` – Assessment-Komposition
- `knowledgeRuntime()` – Knowledge-Komposition
- `ruleService()` – persistenter Rule-Service
- `publicationRuntime()` – Publishing-Komposition

## Designentscheidung

Es wird keine neue Business-Logik in der Runtime implementiert. Die Runtime verdrahtet ausschließlich bestehende Domain-/Application-/Service-Grenzen mit den Execution-Adaptern.

Für Rules existiert im aktuellen Produktstand kein eigener Rule-Runtime-Wrapper. Deshalb wird die bestehende File-Persistence direkt mit `PersistentRuleService` komponiert.

## Testabdeckung

Die Tests prüfen:

1. Registrierung aller vier Adapter.
2. Dispatch eines Assessment Steps.
3. Dispatch eines Knowledge Steps.
4. Dispatch eines Rule Steps.
5. Dispatch eines Publishing Steps.
6. Wiederaufbau der Runtime gegen denselben persistenten Storage.
