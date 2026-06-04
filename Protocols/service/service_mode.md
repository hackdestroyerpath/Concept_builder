# Service Mode

[← Назад к README](../../README.md)

Связанные файлы:
- [Протокол запуска](../common/startup.md)
- [Загрузка контекста](../common/context_loading.md)
- [Обновление state](../common/state_update.md)
- [Жизненный цикл issue](../common/issue_lifecycle.md)
- [Индекс файлов](../../Repository/file_index.jsonl)

## Назначение

`Service Mode` обслуживает сам `Concept Builder`: root README, project instructions, state, protocols, service issue, индексы, граф ссылок и правила восстановления.

## Старт режима

1. Открыть `README.md`.
2. Прочитать `State/service_state.json` и `State/state_schema.md`.
3. Загрузить только `active_protocols`.
4. Восстановить focus, pending action и next step.
5. Ответить с `Health marker`.

## Разрешено

Через утверждённый service issue можно менять `README.md`, `Instructions/`, `State/`, `Protocols/`, `Issues/`, `Repository/`, индексы и правила восстановления.

## Запрещено

- Вести пользовательскую концепцию вместо `Execution Mode`.
- Менять concept files без явного перехода режима.
- Загружать ТЗ-архив, checkpoint-архивы или временные отчёты в рабочий GitHub.
- Создавать новый файл без lean gate.

## Lean gate

Файл создаётся только если он хранит уникальное состояние, протокол, registry, output или navigation; уменьшает контекстную нагрузку; не дублирует source of truth; достижим из README, parent, manifest или file index; имеет понятного читателя и писателя.

## Проверки после изменения

После изменения service-файлов агент проверяет state, file index, граф ссылок, длину project instructions, русский язык и отсутствие временных материалов.
