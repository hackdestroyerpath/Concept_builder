# Inbox

[Назад к README](../README.md)

Связанные файлы:
- [Service issue registry](../Issues/registry.jsonl)
- [Repository file index](../Repository/file_index.jsonl)

## Назначение

`Inbox/` хранит входные материалы, из которых могут появляться service-level или concept-level issue.
Папка конкретного input создаётся только при наличии реального входного материала.

## Правило создания input

Новый input использует путь `Inbox/input_id/` и содержит `entry.md`, `input_manifest.json` и, при необходимости, вложения.
`input_id` должен быть устойчивым и читаемым, например `input_YYYYMMDD_HHMMSS_slug`.

`entry.md` хранит исходный материал или краткое описание источника.
`input_manifest.json` связывает input с registry, вложениями и cleanup status.

## Текущий статус

Активных input folders нет.
Пустые папки не создаются, потому что GitHub не хранит директории без файлов.
