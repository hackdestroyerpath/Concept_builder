# Complex and linked issue

[Назад к issue lifecycle](issue_lifecycle.md)

## Назначение

Primary protocol для complex issue, child issue, linked issue, dependency safeguards, readiness и propagation.

## Связанные файлы

- [Issue lifecycle](issue_lifecycle.md)
- [Input registry](../service/input_registry.md)
- [Execution Mode](../execution/execution_mode.md)

## Complex criteria

Complex issue нужен, если задача имеет independent work units, child approval, разные scopes, dependency chain или отдельный output contract.

## Child workflow

1. Parent фиксирует `decomposition_reason`, `child_candidates`, `parent_acceptance_logic`.
2. Child сначала `proposed`, не `open`.
3. User approves all children or selected children.
4. Approved children become `open`; rejected children get tombstone row with reason.
5. Parent closes only when approved children are closed or waived.

## Relationship schema

```json
{
  "parent_id":null,
  "child_ids":[],
  "blocks":[],
  "depends_on":[],
  "uses_output_of":[],
  "related_to":[],
  "relationship_status":"proposed|approved|satisfied|blocked|tombstoned",
  "propagation_required":true
}
```

## Safeguards

- Maximum depth: 3 unless user approves deeper split.
- Maximum proposed children per pass: 7.
- Dependency graph must be acyclic.
- Child cannot change parent files unless contract allows it.
- Parent cannot close by summary-only output.

## Readiness and propagation

Issue can execute only if dependencies are closed, required outputs exist, parent allows execution and no blocker remains. After closure, agent updates parent, downstream status, output links, registry rows and state files. If propagation fails, closure is blocked.

## Tombstone/link repair

Tombstone keeps identity and reason. References in parent, registry, state, output report and link graph are repaired where applicable.
