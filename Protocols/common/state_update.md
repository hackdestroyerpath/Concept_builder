# Обновление state

[← Назад к README](../../README.md)

Связанные файлы:
- [Схема state](../../State/state_schema.md)
- [Протокол запуска](startup.md)
- [Загрузка контекста](context_loading.md)
- [Жизненный цикл issue](issue_lifecycle.md)

## Назначение

Протокол задаёт правила сохранения state, registry, reason, requirements, solution, contract и output. Нельзя отвечать пользователю “сохранено”, если GitHub-запись не выполнена. Магия, к сожалению, всё ещё не транзакционная база данных.

## Что сохраняется до ответа

| Событие | Обязательные файлы |
|---|---|
| Получены входные данные | `Inbox/<input_id>/entry.md`, `input_manifest.json` |
| Создан proposed issue | строка registry, `state.json`, `reason.md` |
| Созданы requirements | `requirements.md`, issue state |
| Создан solution packet | `plan.md`, `solution.md`, `contract.md`, issue state |
| Выполнен contract | `output/report.md`, attachments при наличии, registry, state |
| Изменён граф или манифест | `Repository/file_index.jsonl`, `Repository/link_graph.md` или concept `manifest.jsonl`, `structure.md` |

## Порядок обновления

1. Прочитай актуальный state.
2. Проверь, что текущий turn относится к этому focus.
3. Измени только нужные поля.
4. Увеличь `state_revision`.
5. Обнови `last_persisted_at`.
6. Пересчитай `state_hash`.
7. Сохрани файл в GitHub.
8. Только после успешной записи отвечай `persisted=yes`.

## Буквальное зеркало reason.md

Полный блок `Reason`, показанный пользователю в чате, и содержимое `reason.md` должны совпадать один в один. Если совпадение не подтверждено, агент не просит пользователя утверждать registry.

## Ошибка записи

Если GitHub-запись не выполнена, агент показывает `persisted=no`, перечисляет несохранённые файлы и не продолжает выполнение solution или export.
