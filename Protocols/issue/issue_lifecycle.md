# Issue lifecycle

[Назад к README](../../README.md)

## Назначение

Primary protocol для service-level and concept-level issue: resume, reason, QA, requirements, plan, solution, contract, execution, output/report and closure. Этот файл задаёт workflow; [complex_linked.md](complex_linked.md) расширяет его для child/dependency cases.

## Связанные файлы

- [Service Mode](../service/service_mode.md)
- [Execution Mode](../execution/execution_mode.md)
- [Input registry](../service/input_registry.md)
- [Complex linked issue](complex_linked.md)
- [Issue template](../../Templates/issue/README.md)

## Issue folder

```text
state.json
reason.md
qa.md                  # only if real questions are needed
requirements.md
plan.md
solution.md
contract.md
output/report.md
output/attachments/    # only if attachments exist
```

`output/report.md` является единственным допустимым именем отчёта. `output_report.md` считается broken template path.

## Registry and resume

Resume starts from registry row, not from memory. Agent opens:

1. relevant registry row;
2. issue `state.json`;
3. `reason.md`;
4. current phase file;
5. parent/child/linked rows if present;
6. focus packet.

If registry row and state disagree, issue status becomes `blocked: registry_state_conflict` until repaired.

## Reason and mirror gate

`reason.md` is canonical stored reason. If issue is created from user-facing proposal, response reason and `reason.md` must match byte-for-byte. If mismatch appears, execution and closure are blocked until mirror is repaired.

## QA gate

`qa.md` is created only if real questions are needed. If not needed, state and requirements record:

```yaml
qa_required: false
qa_decision_reason: "..."
qa_file_created: false
```

If QA is required, requirements cannot be approved until questions are answered or explicitly waived by user.

## Requirements gate

`requirements.md` contains:

```yaml
status: missing|draft|approved
source_reason: path
acceptance_criteria: []
out_of_scope: []
unknowns: []
user_approval: required|received|waived_with_reason
```

Plan may start only after `requirements_status=approved`, except for explicit discovery-only issue where output is questions, not production mutation.

## Plan, solution and contract gates

| File | Minimum contents | Gate |
|---|---|---|
| `plan.md` | steps, affected files, validation plan, rollback/repair note | approved before solution |
| `solution.md` | selected approach, alternatives rejected, exact file operations | approved before contract |
| `contract.md` | allowed files, blocked files, persistence order, success/failure evidence | approved before execution |

Execution without known affected files and persistence plan is forbidden.

## Execution gate

Before editing files:

```yaml
requirements_approved: true
plan_approved: true
solution_approved: true
contract_approved: true
affected_files_known: true
blocked_files_respected: true
persistence_order_known: true
validation_plan_known: true
```

Atomic repair exception must record reason, files, checks and state/output evidence.

## Output/report schema

`output/report.md` contains:

```yaml
issue_id: string
scope: service|concept
reason_path: path
commit_sha: string|null
changed_files: []
registry_updated: true|false
state_updated: true|false
checks_run: []
link_orphan_result: pass|fail|not_applicable
language_gate: pass|fail|not_applicable
residual_risks: []
closure_allowed: true|false
next_expected_step: string
```

Attachments go under `output/attachments/` only if needed and must be referenced from report.

## Closure transitions

```text
validating -> closed       only if output verified and propagation done
validating -> blocked      if persistence/check failed
closed -> open             only via separate repair issue
open|approved -> tombstoned if user rejects or supersedes before execution
```

Closure requires registry row, issue state, output/report, parent-child propagation, link/orphan check and persistence verification.

## Requalification

Issue requalifies when simple becomes complex, service becomes concept, child issue or dependency appears, requirements scope changes, output affects another issue, or affected files cross mode boundaries. Requalification updates registry row, state, focus packet, and if needed delegates to [complex_linked.md](complex_linked.md).

## Concept issue variation

Concept issue follows the same gates but allowed mutations stay inside `Concepts/<slug>/`, except `State/execution_index_state.json` when active concept summary changes. Manifest, structure, local registry and concept state updates are hard gates for page creation/deletion/rename.
