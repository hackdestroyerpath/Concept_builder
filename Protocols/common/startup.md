# Протокол запуска

[Назад к README](../../README.md)

## Назначение

Общий протокол запуска для `Service Mode` и `Execution Mode`: загрузить минимальный контекст, проверить ожидаемое действие пользователя, восстановить фокус и вернуть маркер здоровья.

## Связанные файлы

- [Пакет фокуса](focus_packet.md)
- [Загрузка контекста](context_loading.md)
- [Обновление состояния](state_update.md)
- [Состояние service](../../State/service_state.json)
- [Состояние execution](../../State/execution_index_state.json)

## Алгоритм запуска

1. Определить режим из пользовательского запроса или инструкции проекта.
2. Открыть `README.md`, `Repository/file_index.jsonl`, `Repository/link_graph.md`.
3. Открыть `State/service_state.json` для `Service Mode` или `State/execution_index_state.json` для `Execution Mode`.
4. Проверить `pending_user_action`. Если он не `null`, обычный процесс блокируется и ответ сначала показывает ожидаемое действие.
5. Собрать пакет фокуса по [focus_packet.md](focus_packet.md).
6. Открыть только `active_protocols` и разрешённые файлы фокуса.
7. Если `context_confidence=low`, выполнить восстановление до любых изменений файлов.
8. Если запуск меняет состояние, сохранить его по [state_update.md](state_update.md).
9. Ответить коротким статусом, доступными действиями и маркером здоровья.

## Случаи запуска execution

- `no_active`: активная концепция отсутствует; доступные действия — создать концепцию или открыть `Concepts/root.md`.
- `active_known`: загрузить состояние концепции, README, manifest, structure и локальный registry.
- `active_unknown`: не гадать; выполнить восстановление фокуса и обновить состояние execution.

## Случаи запуска service

- `no_active_issue`: принять новый служебный вход или ждать запроса.
- `active_issue`: открыть registry, состояние задачи и файлы жизненного цикла.
- `state_conflict`: остановить процесс и восстановить registry/state.

## Договор ответа

Ответ запуска содержит:

```yaml
mode: service|execution
loaded_state: path
loaded_protocols: []
pending_user_action: string|null
available_actions: []
health_marker: "mode=<mode>; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>"
```

`persisted=yes` допустимо только если запись действительно прошла через GitHub Connector.
