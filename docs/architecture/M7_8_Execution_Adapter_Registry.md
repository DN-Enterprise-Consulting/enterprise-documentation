# M7.8 – Execution Adapter Registry

## Ziel

M7.8 stellt die zentrale Registry für Execution Adapter bereit. Die Registry entkoppelt die spätere Ausführungsschicht von konkreten Adapter-Implementierungen und löst Adapter über einen stabilen `domainKey` auf.

## Architektur

- `ExecutionAdapter` bleibt der bestehende SPI-Vertrag.
- `ExecutionAdapterRegistry` bleibt der bestehende SPI-Vertrag.
- `InMemoryExecutionAdapterRegistry` ist die konkrete, thread-sichere Runtime-Implementierung.
- Adapter werden über `domainKey()` registriert.
- Ein Domain-Key darf nur einmal registriert werden.
- `findByDomainKey()` liefert ein `Optional`.
- `requireByDomainKey()` nutzt den bestehenden Default-Vertrag und liefert bei fehlender Registrierung eine `IllegalArgumentException`.
- `all()` liefert eine deterministisch nach Domain-Key sortierte Liste.

## Verantwortungsgrenze

M7.8 führt noch keine Cross-Domain-Orchestration aus. Die Registry entscheidet ausschließlich, welcher Execution Adapter für einen Domain-Key zuständig ist. Die eigentliche Ablaufsteuerung über Orchestration Steps bleibt ein nachgelagerter Schritt.

## Bekannte Adapter

Die bisher implementierten Adapter verwenden folgende Domain-Keys:

- `assessment`
- `knowledge`
- `rules`
- `publishing`

M7.8 selbst benötigt keine fachliche Abhängigkeit auf diese konkreten Adapter. Dadurch bleibt die Registry generisch.

## Tests

Abgedeckt werden:

1. Registrierung und Auflösung eines Adapters
2. Duplikat-Schutz für Domain-Keys
3. deterministische Reihenfolge von `all()`
4. Verhalten bei unbekanntem Domain-Key
