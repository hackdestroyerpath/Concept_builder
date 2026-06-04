# Экспорт концепции

[← Назад к Execution Mode](execution_mode.md)

Связанные файлы:
- [Execution Mode](execution_mode.md)
- [Связанные issue](../common/linked_issues.md)
- [Шаблон концепции](../../Concepts/_template/README.md)
- [Граф ссылок](../../Repository/link_graph.md)

## Назначение

Export превращает концепцию в ZIP-архив со связанной сетью Markdown-файлов. Это не “заархивировать папку и надеяться”, хотя индустрия, устав от причинности, любит именно так.

## Команды

| Команда | Действие |
|---|---|
| `проверить export: <concept_slug>` | pre-export проверка без архива |
| `экспорт черновика концепции: <concept_slug>` | draft export с открытыми issue |
| `экспорт концепции: <concept_slug>` | final export после всех gates |
| `обновить manifest: <concept_slug>` | пересобрать manifest и structure |

## Preconditions финального export

- concept существует в `Concepts/<concept_slug>/`;
- `README.md` является точкой входа;
- обязательные страницы существуют или явно не нужны;
- `manifest.jsonl` и `structure.md` актуальны;
- blocking concept issue закрыты;
- граф ссылок пройден;
- русский язык проверен;
- пользователь подтвердил export.

## Состав final archive

```text
concept_<concept_slug>_export_<YYYYMMDD_HHMMSS>.zip
├── README.md
├── about.md
├── operating_model.md
├── requirements.md
├── process.md
├── pages/
├── manifest.jsonl
├── structure.md
└── export_report.md
```

## Draft export

Черновой архив добавляет `open_issues/open_issues.md` и `open_issues/registry_snapshot.jsonl`. Он обязан явно показывать, какие issue мешают final closure.

## Проверка графа

Агент проверяет достижимость всех MD-файлов из concept `README.md`, parent links, наличие manifest entries, отсутствие broken links и работу relative links после распаковки.

## Export report

Report фиксирует тип export, имя архива, preconditions, included files, link validation, open issues, residual risks и next action.
