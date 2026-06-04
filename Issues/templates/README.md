# Шаблоны issue

[← Назад к Issues](../README.md)

Связанные файлы:
- [Жизненный цикл issue](../../Protocols/common/issue_lifecycle.md)
- [Active service issue](../active/README.md)

## Назначение

Папка хранит компактные шаблоны для создания `state.json`, `reason.md`, `requirements.md`, `plan.md`, `solution.md`, `contract.md` и `output/report.md` внутри реального issue.

## Минимальный комплект реального issue

```text
<issue_id>/
├── state.json
├── reason.md
├── qa.md                 # только если QA нужен
├── requirements.md
├── plan.md
├── solution.md
├── contract.md
└── output/report.md
```

## Правило использования

Шаблон не является активным issue. При создании issue агент копирует структуру, заменяет placeholders, записывает registry row и сохраняет файлы до ответа пользователю.
