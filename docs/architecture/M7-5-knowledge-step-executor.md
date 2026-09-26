# M7.5 – Knowledge Step Executor

## Scope
Adds the Knowledge execution adapter to the execution module.

## Boundary
The adapter depends on the existing `PersistentKnowledgeService` service boundary from the Knowledge domain. No repository or persistence implementation is used by the production adapter.

A dedicated Knowledge application facade is not part of the currently implemented Knowledge module baseline, so M7.5 does not invent a new API facade solely for the executor.

## Context Keys
- `knowledge.type`
- `knowledge.name`
- `knowledge.description`
- `knowledge.content`

## Result Outputs
- `knowledgeId`
- `knowledgeType`
- `knowledgeStatus`
- `knowledgeVersion`

## Execution
1. Read and validate required context values.
2. Create a Knowledge Object in DRAFT.
3. Activate the object.
4. Return a successful execution result.
