# Service issue

[← Назад к README](../README.md)

Связанные файлы:
- [Жизненный цикл issue](../Protocols/common/issue_lifecycle.md)
- [Связанные issue](../Protocols/common/linked_issues.md)
- [Service state](../State/service_state.json)
- [Registry](registry.jsonl)
- [Active](active/README.md)
- [Templates](templates/README.md)

## Назначение

Папка хранит service issue, которые обслуживают сам `Concept Builder`. Concept issue живут внутри конкретной концепции, а не здесь.

## Правила

- Issue без `reason.md` не создаётся.
- Полный `Reason` в чате и `reason.md` совпадает побуквенно.
- `requirements.md` создаётся до solution всегда.
- Выполнение начинается только после approved `plan.md`, `solution.md` и `contract.md`.
- Closure требует `output/report.md`, обновлённый state и registry.

## Структура

```text
Issues/
├── registry.jsonl
├── active/
│   └── README.md
└── templates/
    └── README.md
```
