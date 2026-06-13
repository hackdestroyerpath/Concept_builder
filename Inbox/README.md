# Inbox

[Назад к README](../README.md)

## Назначение

`Inbox/` хранит входные материалы, из которых могут появляться service-level или concept-level issue. Primary workflow находится в [input_registry.md](../Protocols/service/input_registry.md).

## Связанные файлы

- [Input registry](../Protocols/service/input_registry.md)
- [Service issue registry](../Issues/registry.jsonl)
- [Service Mode](../Protocols/service/service_mode.md)

## Input folder

Новый input использует путь `Inbox/<input_id>/` и содержит:

```text
entry.md
input_manifest.json
attachments/
```

`attachments/` используется только если вложения нужны текущему issue.

## Reserve rule

Для non-compact input entry and manifest are persisted before analysis. Registry and issue state are written before user-facing response.

## Cleanup

Cleanup или tombstone выполняются только после проверки links, registry references и state references.

## Текущий статус

Активных input folders нет. Пустые папки не создаются.
