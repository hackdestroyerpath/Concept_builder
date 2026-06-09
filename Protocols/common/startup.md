# Startup protocol

[Назад к README](../../README.md)

Связанные файлы:
- [State update](state_update.md)
- [State schema](../../State/state_schema.md)
- [Service state](../../State/service_state.json)
- [Execution index state](../../State/execution_index_state.json)

## Назначение

Общий запуск `Concept Builder` для `Service Mode` и `Execution Mode`.

## Алгоритм запуска

1. Определить режим.
2. Открыть `README.md`.
3. Открыть соответствующий верхний state.
4. Проверить `pending_user_action`.
5. Сформировать focus packet по state.
6. Открыть только protocol-файлы из `active_protocols`.
7. Если startup меняет состояние, сохранить state по `state_update.md`.
8. Ответить коротким статусом и health marker.

## Health marker

`mode=<mode>; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>`
