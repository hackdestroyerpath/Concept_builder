# State schema

[Назад к README](../README.md)

## Назначение

Primary source для всех state-файлов `Concept Builder`: top-level service/execution state, concept state, issue state и output/report linkage. Схема нужна для восстановления работы в новом чате без загрузки всего репозитория.

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
| `current_phase` | string | текущая фаза workflow |
| `current_focus` | string/null | текущий объект внимания |
| `current_entity_id` | string/null | id issue/concept/input/export; при no-active допускается `null` |
| `parent_anchor` | string/null | parent issue/concept/root route |
| `focus_stack` | array | цепочка вложенного focus сверху вниз |
| `active_protocols` | array | protocol paths для startup |
| `allowed_context` | array | файлы, доступные сразу |
| `blocked_context` | array | запрет по умолчанию |
| `pending_user_action` | string/null | действие пользователя, которое блокирует обычный ход |
| `next_expected_step` | string | следующий безопасный шаг |
| `last_persisted_at` | ISO-8601 string | время фактической записи |
| `state_revision` | integer | увеличивается при каждом сохранении |
| `state_hash` | string | `sha256:<hex>` по правилу ниже |
| `last_context_bundle_id` | string | последний focus packet/context bundle |
| `context_summary` | string | краткая выжимка текущего состояния |
| `source_files` | array | источники текущего состояния |
| `output_files` | array | файлы, изменённые последней операции |
| `status` | enum | `active`, `waiting_user`, `blocked`, `closed` |

## State hash semantics

`state_hash` не может быть `pending`, пустой строкой или `null` для active state. Значение вычисляется так:

1. взять JSON state;
2. временно удалить поле `state_hash`;
3. сериализовать canonical JSON с сортировкой ключей, UTF-8 и compact separators;
4. посчитать SHA-256;
5. записать строку `sha256:<64 hex>`.

Если hash нельзя вычислить до записи, операция считается незавершённой: state получает `status=blocked`, `pending_user_action="repair_state_hash"`, а агент не сообщает `persisted=yes`.

## Service state

`State/service_state.json` добавляет:

```json
{
  "active_service_issue_id": null,
  "issue_registry_path": "Issues/registry.jsonl",
  "service_mutation_gate": {
    "approved_issue_required": true,
    "allowed_exception": null,
    "evidence_path": null
  },
  "last_input_id": null,
  "cleanup_queue": []
}
```

Service state используется при system-file mutation, input registry, cleanup/tombstone, final validation and service issue recovery. Если registry row и `active_service_issue_id` расходятся, обычная mutation блокируется до repair.

## Execution index state

`State/execution_index_state.json` добавляет:

```json
{
  "active_concept": null,
  "concepts": [],
  "concept_registry_status": "empty|active|conflict",
  "last_concept_action": null,
  "execution_startup_case": "no_active|active_known|active_unknown"
}
```

При `active_known` agent открывает concept state, README, manifest, structure and local registry. При `active_unknown` agent выполняет focus recovery. При `no_active` нельзя создавать demo concept без реального запроса.

## Concept state

Каждая реальная концепция обязана иметь `Concepts/<concept_slug>/state.json`. Минимальная схема:

```json
{
  "state_id": "concept:<slug>",
  "mode": "concept",
  "concept_slug": "<slug>",
  "current_phase": "skeleton|draft|ready|exporting|released|blocked",
  "current_focus": "concept_root|page|issue|export",
  "current_entity_id": "<slug>",
  "active_issue_id": null,
  "readiness_status": "skeleton|in_progress|ready|blocked",
  "export_status": "never_exported|draft_exported|final_exported|blocked",
  "last_export_report": null,
  "last_exported_at": null,
  "last_export_package": null,
  "manifest_path": "Concepts/<slug>/manifest.jsonl",
  "structure_path": "Concepts/<slug>/structure.md",
  "local_issue_registry": "Concepts/<slug>/Issues/registry.jsonl",
  "focus_pointers": [],
  "open_issues_snapshot": []
}
```

Skeleton concept is not ready until required pages, state, manifest, structure, local registry and link network are synchronized.

## Issue state

Issue state используется в `Issues/active/<issue_id>/state.json` или `Concepts/<slug>/Issues/active/<issue_id>/state.json`. Дополнительные поля:

```json
{
  "issue_id": "...",
  "issue_scope": "service|concept",
  "issue_type": "simple|complex|linked|child",
  "registry_path": ".../registry.jsonl",
  "source_input_id": null,
  "status": "proposed|open|qa|requirements_draft|requirements_approved|planned|plan_approved|solution_approved|contract_approved|executing|validating|closed|blocked|tombstoned",
  "parent_id": null,
  "child_ids": [],
  "linked_issue_ids": [],
  "blocks": [],
  "depends_on": [],
  "uses_output_of": [],
  "related_to": [],
  "qa_decision_reason": null,
  "requirements_status": "missing|draft|approved",
  "plan_status": "missing|draft|approved",
  "solution_status": "missing|draft|approved",
  "contract_status": "missing|draft|approved",
  "output_status": "missing|draft|ready|verified|failed",
  "affected_files": [],
  "allowed_files": [],
  "blocked_files": []
}
```

Запрещены прямые переходы к execution без approved requirements, plan, solution and contract, кроме atomic repair exception с записанным reason and evidence.

## Output/report state linkage

`output/report.md` является читаемым отчётом. Issue state хранит машинную связку:

```json
{
  "output_report_path": ".../output/report.md",
  "output_status": "draft|ready|verified|failed",
  "commit_sha": "...",
  "changed_files": [],
  "checks": [],
  "link_orphan_result": "pass|fail",
  "language_gate": "pass|fail",
  "residual_risks": [],
  "closure_allowed": false
}
```

`output_report.md` запрещён. Единственный путь — `output/report.md`.

## Recovery rules

Если `state_hash` invalid, `current_entity_id` нужен, но отсутствует, active state file не открывается, `state_revision_loaded` не совпадает или registry/manifest конфликтует со state, агент обязан:

1. остановить обычный workflow;
2. собрать focus packet с `context_confidence=low`;
3. открыть README, file index, link graph, relevant state and primary protocol;
4. восстановить missing fields или поставить `pending_user_action`;
5. сохранить relevant state перед продолжением.
