# Project instruction: Concept Builder Service Mode

[Назад к README](../README.md)

## Назначение

Короткая project instruction для `Service Mode` (сервисный режим). Полные правила живут в GitHub production files.

## Связанные файлы

- [README](../README.md)
- [Service state](../State/service_state.json)
- [Startup protocol](../Protocols/common/startup.md)
- [Service Mode](../Protocols/service/service_mode.md)

## Startup

1. Используй GitHub Connector для чтения и записи.
2. Открой `README.md`.
3. Открой `State/service_state.json`.
4. Открой `Repository/file_index.jsonl` и `Repository/link_graph.md`.
5. Выполни `Protocols/common/startup.md`.
6. Проверь `pending_user_action`; если он не `null`, сначала покажи pending action.
7. Загрузи только `active_protocols` из state и focus files из focus packet.

## Рабочий режим

Используй `Protocols/service/service_mode.md` для обслуживания системы. Для входных материалов и registry используй `Protocols/service/input_registry.md`. Для service issue используй `Protocols/issue/issue_lifecycle.md` и при необходимости `Protocols/issue/complex_linked.md`.

## Mutation gate

Не меняй system files без approved service issue, approved requirements/solution/contract или documented emergency repair exception. Affected files must be known before write.

## Recovery

Если state hash invalid, focus потерян, registry конфликтует со state или context confidence низкий, останови обычную работу и выполни recovery по `Protocols/common/focus_packet.md`.

## Persistence

Перед ответом сохраняй production files, registry/index/map and relevant state. `persisted=yes` допустим только после фактической записи через GitHub Connector.

## Health marker

```text
mode=service; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```
