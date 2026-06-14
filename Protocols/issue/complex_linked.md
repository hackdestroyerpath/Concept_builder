# Complex and linked issue

[Назад к issue lifecycle](issue_lifecycle.md)

## Назначение

Основной protocol для complex issue, child issue, linked issue, dependency safeguards, readiness и propagation.

## Связанные файлы

- [Issue lifecycle](issue_lifecycle.md)
- [Input registry](../service/input_registry.md)
- [Execution Mode](../execution/execution_mode.md)

## Complex criteria

Complex issue нужен, если задача имеет independent work units, child approval, разные scopes, dependency chain, separate output contract, partial approval risk или recursion risk.

## Parent issue fields

Parent state добавляет:

```json
{
  "decomposition_reason": "...",
  "child_candidates": [],
  "approved_child_ids": [],
  "rejected_child_ids": [],
  "parent_acceptance_logic": "all_children_closed|selected_children_closed|manual_parent_review",
  "summary_propagation_required": true
}
```

## Child approval workflow

1. Parent предлагает child candidates как `proposed` rows, не как `open` rows.
2. User может approve all, approve selected, reject selected, discuss или edit child candidates.
3. Approved children переходят в `open` и получают state/reason/requirements skeleton.
4. Rejected children получают tombstone row с reason.
5. Parent может execute только части, не заблокированные children; parent closes только когда approved children closed, waived or explicitly superseded.

## Relationship schema

```json
{
  "parent_id": null,
  "child_ids": [],
  "blocks": [],
  "depends_on": [],
  "uses_output_of": [],
  "related_to": [],
  "relationship_status": "proposed|approved|satisfied|blocked|tombstoned",
  "propagation_required": true,
  "propagation_targets": []
}
```

## Linked issue readiness

Issue может execute только если:

```yaml
dependencies_closed_or_waived: true
required_outputs_exist: true
no_cycle_in_dependency_graph: true
parent_allows_execution: true
blocking_children: []
contract_allows_cross_file_change: true
```

Если linked issue uses output of another issue, output/report path и commit SHA должны быть записаны before execution. То есть dependent issue не стартует, пока required output не сохранён и не проверен.

## Safeguards

- Maximum depth: 3 unless user approves deeper split.
- Maximum proposed children per pass: 7.
- Dependency graph must be acyclic.
- Child cannot change parent files unless contract allows it.
- Parent cannot close by summary-only output.
- Rejected child keeps tombstone; deletion without trace is forbidden.
- Cross-mode mutation escalates to service issue.

## Propagation after closure

После child/linked closure agent обновляет:

1. child output/report;
2. child state;
3. parent summary и state;
4. downstream issue state, если `blocks` или `uses_output_of` изменились;
5. registry rows;
6. link graph/manifest, если files changed.

Если propagation fails, closure блокируется со status `blocked: propagation_failed`.

## Dry-run example

```yaml
parent: svc_parent
children_proposed: [svc_child_a, svc_child_b]
user_decision: approve svc_child_a, reject svc_child_b
result:
  svc_child_a: open
  svc_child_b: tombstoned
  parent_acceptance_logic: selected_children_closed
  propagation_required: true
```
