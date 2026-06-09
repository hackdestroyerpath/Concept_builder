# Concept Builder

## Назначение

`Concept Builder` — рабочий репозиторий для ведения концепций через контролируемые Markdown-файлы, state-файлы, registry и issue-процессы.
Система разделяет два режима: `Service Mode` обслуживает саму систему, а `Execution Mode` ведёт пользовательские концепции.
Один смысловой объект должен иметь один primary source, остальные файлы только ссылаются на него или дают короткое summary.

## Быстрый старт агента

1. Открыть этот `README.md` как точку входа.
2. Проверить текущую карту файлов: [Repository/file_index.jsonl](Repository/file_index.jsonl).
3. Проверить достижимость рабочих файлов: [Repository/link_graph.md](Repository/link_graph.md).
4. Для service-level задач смотреть [Issues/registry.jsonl](Issues/registry.jsonl).
5. Для входных материалов использовать [Inbox/README.md](Inbox/README.md).
6. Для пользовательских концепций использовать [Concepts/root.md](Concepts/root.md).

Команды `старт`, `пинг` и `1` загружают минимальный контекст: этот файл, file index, link graph и relevant state.
Полный startup protocol будет закреплён в `Protocols/common/startup.md`.

## Режимы работы

| Режим | Назначение | Primary source | Статус |
|---|---|---|---|
| `Service Mode` | Обслуживает саму систему, registry, протоколы и service issues. | `Protocols/service/service_mode.md` | план Phase 4 |
| `Execution Mode` | Создаёт, ведёт и экспортирует пользовательские концепции. | `Protocols/execution/execution_mode.md` | план Phase 5 |

## Плановые primary sources

| Путь | Назначение | План |
|---|---|---|
| `Instructions/concept_builder_project_instruction.md` | Bootstrap-инструкция для `Execution Mode`. | Phase 2 |
| `Instructions/concept_builder_service_mode_project_instruction.md` | Bootstrap-инструкция для `Service Mode`. | Phase 2 |
| `State/service_state.json` | Верхний state обслуживания системы. | Phase 2 |
| `State/execution_index_state.json` | Верхний state пользовательских концепций. | Phase 2 |
| `State/state_schema.md` | Схема state-файлов. | Phase 2 |
| `Protocols/common/startup.md` | Общий startup protocol. | Phase 3 |
| `Protocols/common/context_loading.md` | Правила focus packet и локальной загрузки контекста. | Phase 3 |
| `Protocols/common/state_update.md` | Правила persistence state. | Phase 3 |
| `Protocols/service/service_mode.md` | Рабочий процесс `Service Mode`. | Phase 4 |
| `Protocols/execution/execution_mode.md` | Рабочий процесс `Execution Mode`. | Phase 5 |

## Repository map

| Файл | Назначение |
|---|---|
| [Repository/file_index.jsonl](Repository/file_index.jsonl) | Машинный индекс рабочих файлов репозитория. |
| [Repository/link_graph.md](Repository/link_graph.md) | Читаемая карта ссылок и orphan-проверка. |
| [Issues/registry.jsonl](Issues/registry.jsonl) | Пустой service issue registry до первого issue. |
| [Inbox/README.md](Inbox/README.md) | Навигация входных материалов. |
| [Concepts/root.md](Concepts/root.md) | Навигация пользовательских концепций. |

## Правила навигации

- Каждый рабочий Markdown-файл имеет заголовок первого уровня и раздел `Назначение`.
- Каждый рабочий Markdown-файл, кроме корневого `README.md`, содержит ссылку возврата к parent-файлу.
- Новый рабочий файл создаётся только после lean gate: функция, источник истины, parent, достижимость и риск дублей.
- ТЗ-архив, checkpoint-архивы, временные заметки и отчёты реализации не загружаются в рабочий GitHub.

## Проверка целостности

Файл считается активным, если он физически существует, описан в `Repository/file_index.jsonl` и достижим через `README.md` или `Repository/link_graph.md`.
Текущий статус: Phase 1 closed.
