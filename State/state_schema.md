# State schema

[Назад к README](../README.md)

Связанные файлы:
- [Service state](service_state.json)
- [Execution index state](execution_index_state.json)
- [Repository file index](../Repository/file_index.jsonl)

## Назначение

Этот файл задаёт минимальную схему state-файлов `Concept Builder`.
State нужен для восстановления фокуса в новом чате без загрузки всего репозитория.

## Общие поля state

| Поле | Назначение |
|---|---|
| `state_id` | Устойчивый идентификатор state-файла. |
| `mode` | `service` или `execution`. |
| `current_phase` | Текущая фаза работы. |
| `current_focus` | Главный объект внимания агента. |
| `focus_stack` | Стек вложенного фокуса. |
| `active_protocols` | Протоколы, которые нужно открыть при старте. |
| `allowed_context` | Файлы, которые можно читать сразу. |
| `blocked_context` | Материалы, которые нельзя загружать без причины. |
| `pending_user_action` | Ожидаемое действие пользователя или `null`. |
| `next_expected_step` | Следующий допустимый шаг. |
| `last_persisted_at` | Время последнего сохранения state. |
| `state_revision` | Номер ревизии state. |
| `state_hash` | Контрольная строка state. |
| `last_context_bundle_id` | Последний идентификатор focus packet. |
| `context_summary` | Краткая выжимка контекста. |
| `source_files` | Файлы, использованные для текущего состояния. |
| `output_files` | Файлы, созданные или изменённые в текущем шаге. |
| `status` | `active`, `waiting_user`, `blocked` или `closed`. |

## Service state

`State/service_state.json` хранит состояние обслуживания самой системы.
Он указывает текущую фазу обслуживания, relevant context и следующий системный шаг.

## Execution index state

`State/execution_index_state.json` хранит верхний индекс пользовательских концепций.
Пока активных концепций нет, `active_concept` равен `null`, а `concepts` является пустым списком.

## Правила обновления

Relevant state обновляется перед ответом пользователю, если изменились focus, registry, requirements, plan, solution, contract, output, export status или next action.

Если state не удалось сохранить через GitHub Connector, агент не должен делать вид, что persistence выполнен.
