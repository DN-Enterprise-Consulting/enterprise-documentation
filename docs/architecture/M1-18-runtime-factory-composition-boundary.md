# M1.18 – Runtime Factory / Composition Boundary

## Purpose

M1.18 introduces an explicit factory as the composition boundary for creating
the knowledge runtime.

## Design

`KnowledgeRuntimeFactory` is located in the `bootstrap` module and exposes the
single entry point:

```java
KnowledgeRuntimeFactory.create(KnowledgeRuntimeConfiguration configuration)
```

The factory validates the configuration and delegates runtime construction to
the existing `KnowledgeRuntime.create(...)` method.

## Architectural intent

- keep runtime construction in the bootstrap boundary;
- avoid spreading concrete persistence wiring into callers;
- keep configuration technology-neutral;
- provide one explicit composition entry point;
- preserve the existing runtime implementation and public behavior.

## Non-goals

M1.18 does not introduce:

- Spring or another dependency-injection framework;
- environment-variable parsing;
- YAML/properties configuration;
- database infrastructure;
- REST endpoints;
- service discovery;
- dependency-injection containers.

## Definition of Done

- factory exists in `bootstrap`;
- factory accepts `KnowledgeRuntimeConfiguration`;
- null configuration is rejected;
- runtime can be created through the factory;
- tests cover successful creation and invalid input;
- no new external runtime framework is introduced.
