# Inbox

[← Назад к README](../README.md)

Связанные файлы:
- [Жизненный цикл issue](../Protocols/common/issue_lifecycle.md)
- [Обновление state](../Protocols/common/state_update.md)
- [Шаблон entry](template/entry.md)
- [Шаблон manifest](template/input_manifest.json)
- [Attachments](template/attachments/README.md)

## Назначение

`Inbox/` хранит входные данные пользователя до превращения в issue registry. Это рабочий буфер с сохранением в GitHub.

## Структура

```text
Inbox/<input_id>/
├── entry.md
├── input_manifest.json
└── attachments/
```

## Правила

- Текст пользователя или явный файл становится `entry.md`.
- Вложения фиксируются в manifest и получают роль: source, context, reference или unknown.
- Registry создаётся только после сохранения input.
- Если вход слишком большой, агент сохраняет input и продолжает registry generation следующим ограниченным шагом.
