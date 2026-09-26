# M7.6 – Rule Evaluation Step Executor

## Scope
Adds the Rules execution adapter to the execution module.

## Boundary
The adapter depends on the existing `PersistentRuleService` service boundary from the Rules domain. No repository or persistence implementation is used by the production adapter.

The current Rules baseline provides rule lifecycle and persistence services, but no expression evaluation engine. M7.6 therefore establishes the Rules execution boundary by creating and activating a rule and returning its definition in the execution result. Actual expression evaluation is deferred until an evaluator contract exists.

## Context Keys
- `rule.type`
- `rule.name`
- `rule.description`
- `rule.definition`

## Result Outputs
- `ruleId`
- `ruleType`
- `ruleStatus`
- `ruleDefinition`

## Execution
1. Read and validate required context values.
2. Create a Rule in DRAFT.
3. Activate the rule.
4. Return a successful execution result containing the rule identity, type, status and definition.
