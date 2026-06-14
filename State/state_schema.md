# Схема состояния

[Назад к README](../README.md)

## Назначение

Основной источник для всех state-файлов `Concept Builder`: верхнего service/execution state, concept state, issue state и связи output/report. Схема нужна для восстановления работы в новом чате без загрузки всего репозитория.

## Связанные файлы

- [Состояние service](service_state.json)
- [Состояние execution](execution_index_state.json)
- [Пакет фокуса](../Protocols/common/focus_packet.md)
- [Обновление состояния](../Protocols/common/state_update.md)

## Общие обязательные поля

| Поле | Тип | Правило |
|---|---|---|
| `state_id` | string | устойчивый идентификатор state-файла |
| `mode` | enum | `service`, `execution`, `concept`, `issue`, `output` |
| `current_phase` | string | текущая фаза рабочего процесса |
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
| `last_context_bundle_id` | string | последний пакет контекста |
| `context_summary` | string | краткая выжимка текущего состояния |
| `source_files` | array | источники текущего состояния |
| `output_files` | array | файлы, изменённые последней операции |
| `status` | enum | `active`, `waiting_user`, `blocked`, `closed` |

## Правило state_hash

`state_hash` не может быть `pending`, пустой строкой или `null` для active state. Значение вычисляется так: взять JSON, временно удалить `state_hash`, сериализовать canonical JSON с сортировкой ключей и compact separators, посчитать SHA-256 и записать `sha256:<64 hex>`.

Если hash нельзя вычислить до записи, операция незавершённа: state получает `status=blocked`, `pending_user_action="repair_state_hash"`, а агент не сообщает `persisted=yes`.

## Service state

`State/service_state.json` добавляет `active_service_issue_id`, `issue_registry_path`, `service_mutation_gate`, `last_input_id` и `cleanup_queue`. Поле `service_mutation_gate.allowed_exception` возвращается в `null` после завершённого ремонта.

Service state используется при изменении системных файлов, input registry, cleanup/tombstone, финальной validation и восстановлении service issue. Если registry row и `active_service_issue_id` расходятся, обычная mutation блокируется до ремонта.

## Execution index state

`State/execution_index_state.json` добавляет `active_concept`, `concepts`, `concept_registry_status`, `last_concept_action` и `execution_startup_case`. При `active_known` агент открывает concept state, README, manifest, structure и local registry. При `active_unknown` агент выполняет focus recovery. При `no_active` нельзя создавать demo concept без реального запроса.

## Concept state

Каждая реальная концепция обязана иметь `Concepts/<concept_slug>/state.json`. Concept state хранит `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `last_exported_at`, `last_export_package`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers` и `open_issues_snapshot`.

Skeleton concept не считается ready, пока required pages, state, manifest, structure, local registry и link network не синхронизированы; эти токены обозначают обязательные страницы, состояние, реестр и сеть ссылок.

## Issue state

Issue state используется в `Issues/active/<issue_id>/state.json` или `Concepts/<slug>/Issues/active/<issue_id>/state.json`. Он хранит id задачи, scope, type, registry path, source input, status, parent/child links, dependencies, статусы requirements/plan/solution/contract/output, affected files, allowed files и blocked files.

Прямые переходы к execution запрещены без approved requirements, plan, solution и contract, кроме atomic repair exception с записанными reason и evidence.

## Связь output/report со state

`output/report.md` является читаемым отчётом. Issue state хранит `output_report_path`, `output_status`, `commit_sha`, `changed_files`, `checks`, `link_orphan_result`, `language_gate`, `residual_risks` и `closure_allowed`.

`output_report.md` запрещён. Единственный путь — `output/report.md`.

## Правила восстановления

Если `state_hash` invalid, `current_entity_id` нужен, но отсутствует, active state file не открывается, `state_revision_loaded` не совпадает или registry/manifest конфликтует со state, агент обязан остановить обычный workflow, собрать focus packet с `context_confidence=low`, открыть README, file index, link graph, relevant state и primary protocol, восстановить missing fields или поставить `pending_user_action`, затем сохранить relevant state перед продолжением. Английские токены в этом предложении являются именами полей и протоколов.
