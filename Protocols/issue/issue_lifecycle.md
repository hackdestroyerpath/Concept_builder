# Жизненный цикл issue

[Назад к README](../../README.md)

## Назначение

Основной protocol для service-level и concept-level issue: resume, reason, QA, requirements, plan, solution, contract, execution, output/report и closure. Этот файл задаёт workflow; [complex_linked.md](complex_linked.md) расширяет его для child/dependency cases.

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
qa.md                  # только если нужны реальные вопросы
requirements.md
plan.md
solution.md
contract.md
output/report.md
output/attachments/    # только если вложения действительно нужны
```

`output/report.md` является единственным допустимым именем отчёта. `output_report.md` считается broken template path.

## Registry и resume

Resume начинается с registry row, а не с памяти. Agent открывает:

1. relevant registry row;
2. issue `state.json`;
3. `reason.md`;
4. current phase file;
5. parent/child/linked rows, если они есть;
6. focus packet.

Если registry row и state расходятся, issue получает status `blocked: registry_state_conflict` до repair.

## Reason и mirror gate

`reason.md` — canonical stored reason (каноническая сохранённая причина). Если issue создан из user-facing proposal, response reason и `reason.md` должны совпадать byte-for-byte. При mismatch execution и closure блокируются до repair mirror.

## QA gate

`qa.md` создаётся только если нужны реальные вопросы. Если вопросы не нужны, state и requirements фиксируют:

```yaml
qa_required: false
qa_decision_reason: "..."
qa_file_created: false
```

Если QA required, requirements нельзя approve до ответа на вопросы или явного user waiver.

## Requirements gate

`requirements.md` содержит:

```yaml
status: missing|draft|approved
source_reason: path
acceptance_criteria: []
out_of_scope: []
unknowns: []
user_approval: required|received|waived_with_reason
```

Plan может стартовать только после `requirements_status=approved`, кроме explicit discovery-only issue, где output — это вопросы, а не production mutation.

## Plan, solution и contract gates

| File | Минимальное содержание | Gate |
|---|---|---|
| `plan.md` | steps, affected files, validation plan, rollback/repair note | approved before solution |
| `solution.md` | selected approach, alternatives rejected, exact file operations | approved before contract |
| `contract.md` | allowed files, blocked files, persistence order, success/failure evidence | approved before execution |

Выполнение без known affected files и persistence plan запрещено.

## Execution gate

Перед editing files:

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

Atomic repair exception фиксирует reason, files, checks и state/output evidence. После завершения repair активная service exception должна быть сброшена, если она была использована.

## Output/report schema

`output/report.md` содержит:

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

Attachments размещаются в `output/attachments/` только если нужны и должны быть указаны в report.

## Closure transitions

```text
validating -> closed       only if output verified and propagation done
validating -> blocked      if persistence/check failed
closed -> open             only via separate repair issue
open|approved -> tombstoned if user rejects or supersedes before execution
```

Closure требует registry row, issue state, output/report, parent-child propagation, link/orphan check и persistence verification.

## Requalification

Issue requalifies, когда simple становится complex, service становится concept, появляется child issue или dependency, меняется requirements scope, output влияет на другой issue или affected files переходят через mode boundaries. Requalification обновляет registry row, state, focus packet и при необходимости delegates to [complex_linked.md](complex_linked.md).

## Concept issue variation

Concept issue использует те же gates, но allowed mutations остаются внутри `Concepts/<slug>/`, кроме `State/execution_index_state.json`, если меняется summary активной концепции. Manifest, structure, local registry и concept state updates являются hard gates для page creation/deletion/rename.
