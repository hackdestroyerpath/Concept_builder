# Project instruction: Concept Builder

[Назад к README](../README.md)

## Назначение

Короткая project instruction для `Execution Mode` (исполнительный режим). Полные правила живут в GitHub production files, а не здесь.

## Связанные файлы

- [README](../README.md)
- [Execution index state](../State/execution_index_state.json)
- [Startup protocol](../Protocols/common/startup.md)
- [Execution Mode](../Protocols/execution/execution_mode.md)

## Startup

1. Используй GitHub Connector для чтения и записи.
2. Открой `README.md`.
3. Открой `State/execution_index_state.json`.
4. Открой `Repository/file_index.jsonl` и `Repository/link_graph.md`.
5. Выполни `Protocols/common/startup.md`.
6. Проверь `pending_user_action`; если он не `null`, сначала покажи pending action и не продолжай обычный workflow.
7. Загрузи только `active_protocols` из state и focus files из focus packet.

## Рабочий режим

Используй `Protocols/execution/execution_mode.md` для создания/ведения концепций. Для issue внутри концепции используй `Protocols/issue/issue_lifecycle.md`; для export — `Protocols/release/concept.md`.

## Recovery

Если active concept неизвестен, `current_entity_id` отсутствует, state конфликтует с registry или context confidence низкий, останови обычную работу и выполни recovery по `Protocols/common/focus_packet.md` и `Protocols/common/context_loading.md`.

## Persistence

Перед ответом сохраняй changed concept files, manifest, structure, concept state and `State/execution_index_state.json`, если они изменились. Пиши `persisted=yes` только после фактической записи через GitHub Connector.

## Границы

Не меняй `Protocols/`, `State/state_schema.md`, `Instructions/`, `Repository/` как основную задачу Execution Mode. Для системных изменений переходи в `Service Mode` или создавай service issue.

## Health marker

В конце startup или state-changing ответа дай marker:

```text
mode=execution; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```
