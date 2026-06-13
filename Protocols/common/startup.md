# Startup protocol

[Назад к README](../../README.md)

## Назначение

Общий startup protocol для `Service Mode` и `Execution Mode`: загрузить минимальный контекст, проверить pending action, восстановить focus и вернуть health marker.

## Связанные файлы

- [Focus packet](focus_packet.md)
- [Context loading](context_loading.md)
- [State update](state_update.md)
- [Service state](../../State/service_state.json)
- [Execution index state](../../State/execution_index_state.json)

## Startup algorithm

1. Определить режим из пользовательского запроса или project instruction.
2. Открыть `README.md`, `Repository/file_index.jsonl`, `Repository/link_graph.md`.
3. Открыть `State/service_state.json` для `Service Mode` или `State/execution_index_state.json` для `Execution Mode`.
4. Проверить `pending_user_action`. Если он не `null`, обычный workflow блокируется и ответ должен сначала показать pending action.
5. Собрать focus packet по [focus_packet.md](focus_packet.md).
6. Открыть только `active_protocols` и allowed focus files.
7. Если `context_confidence=low`, выполнить recovery до любых file mutations.
8. Если startup меняет state, сохранить по [state_update.md](state_update.md).
9. Ответить коротким статусом, available actions и health marker.

## Execution startup cases

- `no_active`: active concept отсутствует; доступные действия — создать концепцию или открыть `Concepts/root.md`.
- `active_known`: загрузить concept state, README, manifest, structure и локальный registry.
- `active_unknown`: не гадать; выполнить focus recovery и обновить execution index state.

## Service startup cases

- `no_active_issue`: принять новый service input или ждать запроса.
- `active_issue`: открыть registry, issue state и lifecycle files.
- `state_conflict`: остановить workflow и выполнить repair registry/state.

## Response contract

Startup response содержит:

```yaml
mode: service|execution
loaded_state: path
loaded_protocols: []
pending_user_action: string|null
available_actions: []
health_marker: "mode=<mode>; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>"
```

`persisted=yes` допустим только если запись действительно прошла через GitHub Connector.
