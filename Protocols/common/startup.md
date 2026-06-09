# Startup protocol

[Назад к README](../../README.md)

Связанные файлы:
- [Context loading](context_loading.md)
- [State update](state_update.md)
- [Service state](../../State/service_state.json)
- [Execution index state](../../State/execution_index_state.json)

## Назначение

Этот протокол задаёт общий запуск `Concept Builder` для `Service Mode` и `Execution Mode`.
Он используется при командах `старт`, `пинг`, `1` и при восстановлении нового чата.

## Алгоритм запуска

1. Определить режим: `Service Mode` или `Execution Mode`.
2. Открыть `README.md`.
3. Открыть соответствующий верхний state.
4. Проверить `pending_user_action`.
5. Сформировать focus packet по `Protocols/common/context_loading.md`.
6. Открыть только protocol-файлы из `active_protocols`.
7. Если startup меняет focus или pending action, сохранить state по `Protocols/common/state_update.md`.
8. Ответить коротким статусом и health marker.

## Ответ после запуска

```text
Режим: Service Mode | Execution Mode
State загружен: да|нет
Текущий фокус: <id|none>
Активная фаза: <phase>
Подгруженные протоколы: <paths>
Доступные действия: <short list>
Health marker: mode=<mode>; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```

Если есть `pending_user_action`, обычное меню не показывается. Сначала показывается ожидаемое действие пользователя.
