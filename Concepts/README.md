# Concepts

[← Назад к README](../README.md)

Связанные файлы:
- [Execution Mode](../Protocols/execution/execution_mode.md)
- [Экспорт концепции](../Protocols/execution/concept_export.md)
- [State исполнения](../State/execution_index_state.json)
- [Шаблон концепции](_template/README.md)

## Назначение

`Concepts/` хранит пользовательские концепции как связанную сеть Markdown-файлов, локальный state, concept issue registry, manifest и structure map.

## Правила

- Одна концепция открывается через `Concepts/<concept_slug>/README.md`.
- Агент не читает все концепции без причины.
- Любое изменение концепции идёт через concept issue.
- `manifest.jsonl` покрывает файлы концепции.
- `structure.md` отражает фактический граф ссылок.
- Export создаёт архив конкретной концепции, а не всего репозитория.

## Шаблон

Минимальный каркас новой концепции находится в [`_template/`](_template/README.md). При создании реальной концепции агент копирует структуру, заменяет placeholders и обновляет [`State/execution_index_state.json`](../State/execution_index_state.json).
