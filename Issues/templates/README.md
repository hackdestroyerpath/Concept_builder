# Шаблоны issue

[← Назад к Issues](../README.md)

Связанные файлы:
- [Жизненный цикл issue](../../Protocols/common/issue_lifecycle.md)
- [Active service issue](../active/README.md)

## Назначение

Папка хранит компактные шаблоны для создания `state.json`, `reason.md`, `qa.md`, `requirements.md`, `plan.md`, `solution.md`, `contract.md` и `output/report.md` внутри реального issue.

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
└── output/
    ├── report.md
    └── attachments/
```

## Файлы шаблона

- [`simple_issue/state.json`](simple_issue/state.json)
- [`simple_issue/reason.md`](simple_issue/reason.md)
- [`simple_issue/qa.md`](simple_issue/qa.md)
- [`simple_issue/requirements.md`](simple_issue/requirements.md)
- [`simple_issue/plan.md`](simple_issue/plan.md)
- [`simple_issue/solution.md`](simple_issue/solution.md)
- [`simple_issue/contract.md`](simple_issue/contract.md)
- [`simple_issue/output/report.md`](simple_issue/output/report.md)
- [`simple_issue/output/attachments/README.md`](simple_issue/output/attachments/README.md)

## Правило использования

Шаблон не является активным issue. При создании issue агент копирует структуру, заменяет placeholders, записывает registry row и сохраняет файлы до ответа пользователю.
