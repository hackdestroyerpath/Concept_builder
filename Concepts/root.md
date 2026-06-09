# Concepts

[Назад к README](../README.md)

Связанные файлы:
- [Repository file index](../Repository/file_index.jsonl)
- [Repository link graph](../Repository/link_graph.md)

## Назначение

`Concepts/` хранит пользовательские концепции, которые ведутся в `Execution Mode`.
Каждая концепция является отдельной связанной Markdown-сетью со своим manifest, structure и локальным issue registry.

## Правило создания концепции

Новая концепция получает путь `Concepts/concept_slug/`.
Точка входа конкретной концепции: `Concepts/concept_slug/README.md`.

Типовой состав concept folder:

- `README.md`
- `about.md`
- `operating_model.md`
- `requirements.md`
- `process.md`
- `pages/`
- `Issues/`
- `manifest.jsonl`
- `structure.md`

`manifest.jsonl` описывает файлы концепции и нужен для orphan-проверки внутри concept scope.
`structure.md` хранит читаемую карту концепции.

## Текущий статус

Активных концепций нет.
Демонстрационные concept folders без реального запроса не создаются.
