# Структура шаблона концепции

[← Назад к шаблону концепции](README.md)

Связанные файлы:
- [Манифест](manifest.jsonl)
- [State](state.json)

## Назначение

`structure.md` показывает карту файлов концепции и используется при проверке export readiness.

## ASCII-карта

```text
Concepts/<concept_slug>/
├── README.md
├── about.md
├── operating_model.md
├── requirements.md
├── process.md
├── pages/README.md
├── Issues/
│   ├── README.md
│   ├── registry.jsonl
│   └── active/README.md
├── manifest.jsonl
├── structure.md
└── state.json
```

## Проверка

- Entry point: `README.md`.
- Все обязательные MD-файлы достижимы из `README.md`.
- Каждая вложенная страница имеет parent link.
- `manifest.jsonl` перечисляет все файлы шаблона.
