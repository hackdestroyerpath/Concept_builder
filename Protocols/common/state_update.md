# State update protocol

[Назад к README](../../README.md)

## Назначение

Primary protocol для сохранения relevant state перед ответом. State нужен для recovery нового чата; текстовое обещание без записи не считается persistence.

## Связанные файлы

- [State schema](../../State/state_schema.md)
- [Focus packet](focus_packet.md)
- [Service state](../../State/service_state.json)
- [Execution index state](../../State/execution_index_state.json)

## Когда обновлять state

State обновляется до ответа, если изменились focus, registry, issue/concept status, requirements, plan, solution, contract, output, export status, active protocols, context rules или next expected step.

## Порядок сохранения

1. Определить relevant state file.
2. Обновить phase, focus, entity id, pending action, next step и status.
3. Синхронизировать `source_files` и `output_files`.
4. Увеличить `state_revision`.
5. Обновить `last_persisted_at` перед записью.
6. Пересчитать `state_hash` по [State schema](../../State/state_schema.md).
7. Записать файл через GitHub Connector.
8. Проверить commit SHA или перечитать файл.
9. Писать `persisted=yes` только после успешной записи.

## Failure behavior

Если запись не выполнена, ответ должен содержать: `Persistence не выполнен: state не сохранён.` После этого issue, export и final check не закрываются как passed.

## Hash coverage

Hash покрывает весь state JSON кроме поля `state_hash`. Значение `pending`, пустая строка и `null` недопустимы для active state.
