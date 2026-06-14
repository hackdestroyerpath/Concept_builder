# Инструкция проекта: Concept Builder

[Назад к README](../README.md)

## Назначение

Короткая инструкция проекта для `Execution Mode` (исполнительного режима). Полные правила живут в рабочих файлах GitHub, а не в этой краткой инструкции.

## Связанные файлы

- [README](../README.md)
- [Состояние execution](../State/execution_index_state.json)
- [Протокол запуска](../Protocols/common/startup.md)
- [Execution Mode](../Protocols/execution/execution_mode.md)

## Запуск

1. Используй GitHub Connector для чтения и записи.
2. Открой `README.md`.
3. Открой `State/execution_index_state.json`.
4. Открой `Repository/file_index.jsonl` и `Repository/link_graph.md`.
5. Выполни `Protocols/common/startup.md`.
6. Проверь `pending_user_action`; если он не `null`, сначала покажи ожидаемое действие и не продолжай обычный процесс.
7. Загрузи только `active_protocols` из состояния и файлы фокуса из пакета фокуса.

## Рабочий режим

Используй `Protocols/execution/execution_mode.md` для создания и ведения концепций. Для задач внутри концепции используй `Protocols/issue/issue_lifecycle.md`; для выпуска — `Protocols/release/concept.md`.

## Восстановление

Если активная концепция неизвестна, `current_entity_id` отсутствует, состояние конфликтует с реестром или уверенность контекста низкая, останови обычную работу и выполни восстановление по `Protocols/common/focus_packet.md` и `Protocols/common/context_loading.md`.

## Сохранение

Перед ответом сохраняй изменённые файлы концепции, manifest, structure, состояние концепции и `State/execution_index_state.json`, если они изменились. Пиши `persisted=yes` только после фактической записи через GitHub Connector.

## Границы

Не меняй `Protocols/`, `State/state_schema.md`, `Instructions/` и `Repository/` как основную задачу `Execution Mode`. Для системных изменений переходи в `Service Mode` или создавай служебную задачу.

## Маркер здоровья

```text
mode=execution; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```
