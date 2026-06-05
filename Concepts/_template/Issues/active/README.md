# Active concept issue

[← Назад к issue концепции](../README.md)

Связанные файлы:
- [Registry](../registry.jsonl)
- [Жизненный цикл issue](../../../../Protocols/common/issue_lifecycle.md)

## Назначение

Здесь создаются активные concept issue: `Concepts/<concept_slug>/Issues/active/<issue_id>/`.

## Структура

```text
<issue_id>/
├── state.json
├── reason.md
├── qa.md
├── requirements.md
├── plan.md
├── solution.md
├── contract.md
└── output/report.md
```

Файлы создаются только после registry row и сохранённого reason.
