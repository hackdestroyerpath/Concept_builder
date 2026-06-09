# State update protocol

[Назад к README](../../README.md)

Связанные файлы:
- [Startup protocol](startup.md)
- [State schema](../../State/state_schema.md)
- [Service state](../../State/service_state.json)
- [Execution index state](../../State/execution_index_state.json)

## Назначение

Этот протокол задаёт правила сохранения relevant state перед ответом пользователю.
State является источником восстановления нового чата, поэтому нельзя утверждать, что состояние сохранено, если запись в GitHub не выполнена.

## Когда обновлять state

State обновляется перед ответом, если изменились:

- focus или focus stack;
- registry;
- статус issue или concept;
- requirements, plan, solution, contract или output;
- export status;
- next expected step;
- active protocols.

## Порядок обновления

1. Определить relevant state file.
2. Обновить `current_phase`, `current_focus`, `pending_user_action`, `next_expected_step` и `status`.
3. Добавить изменённые файлы в `output_files`.
4. Повысить `state_revision`.
5. Обновить `last_persisted_at`.
6. Записать файл через GitHub Connector.
7. В ответе указать `persisted=yes`, только если запись реально выполнена.

## Если запись не удалась

Если GitHub persistence не выполнен, агент останавливает дальнейшую операцию и сообщает:

```text
Persistence не выполнен: state не сохранён.
```

Нельзя имитировать сохранение state в тексте ответа.
