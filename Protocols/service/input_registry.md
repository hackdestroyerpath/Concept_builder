# Input and registry

[Назад к README](../../README.md)

## Назначение

Протокол описывает создание input, работу с Inbox и service issue registry.

## Input

Новый вход сохраняется в `Inbox/<input_id>/`.
Минимальные файлы: `entry.md`, `input_manifest.json`, при необходимости `attachments/`.

## Registry

`Issues/registry.jsonl` хранит service issue rows.
Каждая строка содержит issue id, scope, title, status, type, links, source input, paths, timestamps и next step.

## Reason mirror

Полный Reason в ответе пользователю и `reason.md` должны совпадать побуквенно.

## Commands

Поддерживаются команды: `утверждаю всё`, `утверждаю: ID`, `отклоняю: ID`, `обсудить: ID`, `отложить: ID`, `изменить: ID`, `добавить: title + reason`, `фокус: ID`.

## Persistence

До ответа пользователю input, registry, issue state и reason должны быть записаны в GitHub.
