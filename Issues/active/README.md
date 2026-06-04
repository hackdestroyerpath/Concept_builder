# Active service issue

[← Назад к Issues](../README.md)

Связанные файлы:
- [Registry](../registry.jsonl)
- [Жизненный цикл issue](../../Protocols/common/issue_lifecycle.md)
- [Templates](../templates/README.md)

## Назначение

Здесь создаются активные service issue в формате `Issues/active/<issue_id>/`. Пустая папка в GitHub не хранится, поэтому этот README является навигационной точкой, а не грустной заглушкой ради галочки.

## Структура issue

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

Перед созданием issue агент обновляет `Issues/registry.jsonl`.
