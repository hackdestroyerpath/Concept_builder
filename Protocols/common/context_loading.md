# Загрузка контекста

[Назад к README](../../README.md)

## Назначение

Протокол минимальной загрузки контекста для `Service Mode`, `Execution Mode`, concept и issue. Основная схема находится в [focus_packet.md](focus_packet.md).

## Связанные файлы

- [Пакет фокуса](focus_packet.md)
- [Протокол запуска](startup.md)
- [Индекс файлов](../../Repository/file_index.jsonl)
- [Карта связей](../../Repository/link_graph.md)

## Минимальный запуск

1. `README.md`.
2. `Repository/file_index.jsonl`.
3. `Repository/link_graph.md`.
4. Нужное верхнее состояние.
5. Пути протоколов из `active_protocols`.
6. Файлы фокуса из `allowed_context`.

Всё дерево репозитория, все concepts, все issues и вложения не читаются без конкретной причины в `reload_reason`.

## Экономная проверка

```yaml
file_has_clear_function: true
primary_source_known: true
parent_known: true
reachable_from_entry: true
index_or_manifest_update_known: true
duplicate_risk_checked: true
language_gate_known: true
```

Если проверка не проходит, рабочий файл не создаётся. Для `Concepts/<slug>/` обновляются local manifest, structure, состояние концепции и сеть ссылок.

## Расширение контекста

Расширение разрешено только по причине: `missing_source`, `broken_link`, `state_conflict`, `low_confidence`, `user_request`, `export_precheck`, `issue_resume`. Причина записывается в пакет фокуса.

## Восстановление фокуса

Если состояние загружено, но нельзя определить `current_entity_id`, родительский якорь, активные state-файлы или следующий шаг, работа блокируется до восстановления по [focus_packet.md](focus_packet.md).
