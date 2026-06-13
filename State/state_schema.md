# State schema

[Назад к README](../README.md)

## Назначение

Primary source для всех state-файлов `Concept Builder`: top-level service/execution state, concept state, issue state и output state linkage. Схема нужна для восстановления работы в новом чате без загрузки всего репозитория.

## Связанные файлы

- [Service state](service_state.json)
- [Execution index state](execution_index_state.json)
- [Focus packet](../Protocols/common/focus_packet.md)
- [State update](../Protocols/common/state_update.md)

## Общие обязательные поля

| Поле | Тип | Правило |
|---|---|---|
| `state_id` | string | устойчивый идентификатор state-файла |
| `mode` | enum | `service`, `execution`, `concept`, `issue`, `output` |
| `current_phase` | string | текущая фаза |
| `current_focus` | string/null | текущий объект внимания |
| `current_entity_id` | string/null | id issue/concept/input/export, если есть |
| `parent_anchor` | string/null | parent issue/concept/root route |
| `focus_stack` | array | цепочка вложенного focus |
| `active_protocols` | array | protocol paths для startup |
| `allowed_context` | array | файлы, доступные сразу |
| `blocked_context` | array | запрет по умолчанию |
| `pending_user_action` | string/null | действие пользователя, которое блокирует обычный ход |
| `next_expected_step` | string | следующий безопасный шаг |
| `last_persisted_at` | ISO-8601 string | время фактической записи |
| `state_revision` | integer | увеличивается при каждом сохранении |
| `state_hash` | string | `sha256:<hex>` по правилу ниже |
| `last_context_bundle_id` | string | последний focus packet/context bundle |
| `context_summary` | string | краткая выжимка |
| `source_files` | array | источники текущего состояния |
| `output_files` | array | файлы, изменённые последней операцией |
| `status` | enum | `active`, `waiting_user`, `blocked`, `closed` |

## State hash semantics

`state_hash` не может быть `pending`. Значение вычисляется как `sha256:` + SHA-256 от canonical JSON:

1. взять JSON state;
2. временно удалить поле `state_hash`;
3. сериализовать с сортировкой ключей, UTF-8, без лишних пробелов;
4. посчитать SHA-256;
5. записать строку `sha256:<64 hex>`.

Если hash нельзя вычислить до записи, операция считается незавершённой: state получает `status=blocked`, `pending_user_action="repair_state_hash"`, а агент не сообщает `persisted=yes`.

## Service state

`State/service_state.json` добавляет:

- `active_service_issue_id`;
- `issue_registry_path`;
- `service_mutation_gate` (`approved_issue_required`, `allowed_exception`, `evidence_path`);
- `last_input_id`;
- `cleanup_queue`.

## Execution index state

`State/execution_index_state.json` добавляет:

- `active_concept`;
- `concepts` со ссылками на `Concepts/<slug>/state.json`;
- `concept_registry_status`;
- `last_concept_action`;
- `execution_startup_case`: `no_active`, `active_known`, `active_unknown`.

## Concept state

Каждая реальная концепция обязана иметь `Concepts/<concept_slug>/state.json`. Минимальные поля:

```json
{
  "state_id":"concept:<slug>",
  "mode":"concept",
  "concept_slug":"<slug>",
  "current_phase":"skeleton|draft|ready|exporting|released",
  "active_issue_id":null,
  "readiness_status":"skeleton|in_progress|ready|blocked",
  "export_status":"never_exported|draft_exported|final_exported|blocked",
  "last_export_report":null,
  "manifest_path":"Concepts/<slug>/manifest.jsonl",
  "structure_path":"Concepts/<slug>/structure.md",
  "local_issue_registry":"Concepts/<slug>/Issues/registry.jsonl",
  "focus_pointers":[]
}
```

## Issue state

Issue state используется в `Issues/active/<issue_id>/state.json` или внутри concept scope. Обязательные дополнительные поля:

- `issue_id`, `issue_scope`, `issue_type`;
- `registry_path`;
- `source_input_id`;
- `status`: `proposed`, `open`, `qa`, `requirements_draft`, `requirements_approved`, `planned`, `solution_approved`, `contract_approved`, `executing`, `validating`, `closed`, `blocked`, `tombstoned`;
- `parent_id`, `child_ids`, `linked_issue_ids`, `blocks`, `depends_on`, `uses_output_of`, `related_to`;
- `qa_decision_reason`;
- `requirements_status`, `plan_status`, `solution_status`, `contract_status`, `output_status`;
- `affected_files`, `allowed_files`, `blocked_files`.

## Output/report state linkage

`output/report.md` является читаемым отчётом, а issue state хранит машинную связку:

```json
{
  "output_report_path":".../output/report.md",
  "output_status":"draft|ready|verified|failed",
  "commit_sha":"...",
  "changed_files":[],
  "checks":[],
  "residual_risks":[],
  "closure_allowed":false
}
```

## Recovery rules

Если `current_entity_id`, active state file или `state_revision_loaded` отсутствуют, агент обязан остановить обычную работу, собрать focus packet с `context_confidence=low`, открыть только primary sources и восстановить state перед дальнейшими изменениями.
