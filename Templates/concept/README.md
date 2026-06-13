# Шаблон концепции

[Назад к Execution Mode](../../Protocols/execution/execution_mode.md)

## Назначение

Summary-шаблон минимальной структуры реальной пользовательской концепции. Primary workflow описан в [execution_mode.md](../../Protocols/execution/execution_mode.md).

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
pages/
state.json
Issues/registry.jsonl
manifest.jsonl
structure.md
```

## State и readiness

`state.json` обязателен. Skeleton concept is not ready until README, required pages, manifest, structure, local registry and state are synchronized.

## Link network

Concept closure validates README forward links, child backlinks, relative local-open check, manifest/structure mirror, orphan files, local issue registry and Russian language gate.
