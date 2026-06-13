# Complex and linked issue

[Назад к issue lifecycle](issue_lifecycle.md)

## Назначение

Primary protocol для complex issue, child issue, linked issue, dependency safeguards, readiness and propagation.

## Связанные файлы

- [Issue lifecycle](issue_lifecycle.md)
- [Input registry](../service/input_registry.md)
- [Execution Mode](../execution/execution_mode.md)

## Complex criteria

Complex issue нужен, если задача имеет independent work units, child approval, разные scopes, dependency chain, separate output contract, partial approval risk or recursion risk.

## Parent issue fields

Parent state adds:

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

1. Parent proposes child candidates as `proposed` rows, not `open` rows.
2. User may approve all, approve selected, reject selected, discuss or edit child candidates.
3. Approved children become `open` and receive state/reason/requirements skeleton.
4. Rejected children get tombstone row with reason.
5. Parent can execute only parts not blocked by children; parent closes only when approved children are closed, waived or explicitly superseded.

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

Issue can execute only if:

```yaml
dependencies_closed_or_waived: true
required_outputs_exist: true
no_cycle_in_dependency_graph: true
parent_allows_execution: true
blocking_children: []
contract_allows_cross_file_change: true
```

If linked issue uses output of another issue, output/report path and commit SHA must be recorded before execution.

## Safeguards

- Maximum depth: 3 unless user approves deeper split.
- Maximum proposed children per pass: 7.
- Dependency graph must be acyclic.
- Child cannot change parent files unless contract allows it.
- Parent cannot close by summary-only output.
- Rejected child keeps tombstone; deletion without trace is forbidden.
- Cross-mode mutation escalates to service issue.

## Propagation after closure

After child/linked closure, agent updates:

1. child output/report;
2. child state;
3. parent summary and state;
4. downstream issue state if `blocks` or `uses_output_of` changed;
5. registry rows;
6. link graph/manifest if files changed.

If propagation fails, closure is blocked with `blocked: propagation_failed`.

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
