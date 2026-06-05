# Граф ссылок репозитория

[← Назад к README](../README.md)

Связанные файлы:
- [Индекс файлов](file_index.jsonl)
- [Схема state](../State/state_schema.md)
- [Протокол запуска](../Protocols/common/startup.md)
- [Concepts](../Concepts/README.md)

## Назначение

Карта навигации и базовых проверок рабочей структуры `Concept Builder`. Машинный список файлов хранится в `file_index.jsonl`.

## Основные входы

- `README.md` ведёт в `Instructions/`, `State/`, `Protocols/`, `Issues/`, `Inbox/`, `Concepts/`, `Repository/`.
- `Concepts/README.md` ведёт в `Concepts/_template/README.md`.
- `Concepts/_template/README.md` ведёт в `about.md`, `operating_model.md`, `requirements.md`, `process.md`, `pages/README.md`, `Issues/README.md`, `manifest.jsonl`, `structure.md`, `state.json`.
- `Issues/README.md` ведёт в `registry.jsonl`, `active/README.md`, `templates/README.md`.
- `Inbox/README.md` ведёт в `template/entry.md`, `template/input_manifest.json`, `template/attachments/README.md`.

## Bootstrap check

| Check | Status |
|---|---|
| Root entry exists | pass |
| State files exist | pass |
| Protocol files exist | pass |
| Service issue area exists | pass |
| Inbox area exists | pass |
| Concept template exists | pass |
| File index exists | pass |

## Правила поддержки

- При добавлении файла обновить `file_index.jsonl`.
- При добавлении MD-файла добавить parent link и путь из точки входа.
- При изменении концепции обновить её `manifest.jsonl` и `structure.md`.
- ТЗ-архив, checkpoint-архивы и временные отчёты разработки не добавлять в рабочий GitHub.
