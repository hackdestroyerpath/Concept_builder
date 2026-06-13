# Шаблон концепции

[Назад к Execution Mode](../../Protocols/execution/execution_mode.md)

## Назначение

Summary-шаблон минимальной структуры реальной пользовательской концепции. Primary workflow описан в [execution_mode.md](../../Protocols/execution/execution_mode.md), export workflow — в [concept.md](../../Protocols/release/concept.md).

## Связанные файлы

- [Execution Mode](../../Protocols/execution/execution_mode.md)
- [Concept release](../../Protocols/release/concept.md)
- [Concepts root](../../Concepts/root.md)

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

`state.json` обязателен. Skeleton concept не считается ready, пока README, required pages, manifest, structure, local registry, concept state and link network are synchronized.

## Link network

Concept closure validates README forward links, child backlinks, relative local-open check, manifest/structure mirror, orphan files, local issue registry and Russian language gate.

## Export fields

После draft/final export concept state обновляет `export_status`, `last_export_report`, `last_exported_at`, `last_export_package`, `open_issues_snapshot` and `next_expected_step`.
