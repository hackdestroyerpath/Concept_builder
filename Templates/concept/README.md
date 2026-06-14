# Шаблон концепции

[Назад к Execution Mode](../../Protocols/execution/execution_mode.md)

## Назначение

Краткий шаблон минимальной структуры реальной пользовательской концепции. Основной рабочий процесс описан в [execution_mode.md](../../Protocols/execution/execution_mode.md), а выпуск — в [concept.md](../../Protocols/release/concept.md).

## Связанные файлы

- [Execution Mode](../../Protocols/execution/execution_mode.md)
- [Выпуск концепции](../../Protocols/release/concept.md)
- [Корень Concepts](../../Concepts/root.md)

## Минимальный состав

```text
README.md
about.md
operating_model.md
requirements.md
process.md
state.json
Issues/registry.jsonl
manifest.jsonl
structure.md
pages/                 # только когда есть реальные страницы
```

## State и readiness

`state.json` обязателен. Skeleton concept не считается ready, пока README, required pages, manifest, structure, local registry, concept state и link network не синхронизированы.

## Link network

Закрытие concept проверяет технические проверки: forward links из README, backlinks дочерних pages, relative local-open check, manifest/structure mirror, orphan files, local issue registry и Russian language gate. Смысл: final export запрещён, пока локальная Markdown-сеть не открывается и не проверяется целиком.

## Поля export

После draft/final export concept state обновляет `export_status`, `last_export_report`, `last_exported_at`, `last_export_package`, `open_issues_snapshot` и `next_expected_step`.
