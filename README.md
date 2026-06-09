# Concept Builder

## Что такое Concept Builder

`Concept Builder` — рабочий репозиторий для ведения концепций через контролируемые Markdown-файлы, state-файлы, registry и issue-процессы.
Система разделяет два режима: `Service Mode` обслуживает саму систему, а `Execution Mode` ведёт пользовательские концепции.
Главный принцип: один смысловой объект имеет один primary source, а остальные файлы только ссылаются на него или дают короткое summary.
Репозиторий не хранит ТЗ-архив, временные заметки реализации, checkpoint-архивы или подготовительные материалы.
Любой новый рабочий файл должен иметь назначение, parent-link и запись в `Repository/file_index.jsonl` либо в локальном manifest.
Текущий статус: создан минимальный skeleton Phase 1; протоколы, state и project instructions добавляются следующими фазами.

## Режимы работы

| Режим | Назначение | Primary source | Статус |
|---|---|---|---|
| `Service Mode` | Изменение самой системы, ведение service issue, обслуживание registry и протоколов. | `Protocols/service/service_mode.md` | будет создан в Phase 4 |
| `Execution Mode` | Создание, развитие и export пользовательских концепций. | `Protocols/execution/execution_mode.md` | будет создан в Phase 5 |

До создания protocol-файлов режимы считаются зарезервированными областями, а не готовыми инструкциями.

## Быстрый старт агента

1. Открыть этот `README.md` как точку входа.
2. Проверить текущую карту файлов: [Repository/file_index.jsonl](Repository/file_index.jsonl).
3. Проверить достижимость рабочих файлов: [Repository/link_graph.md](Repository/link_graph.md).
4. Для service-level задач смотреть [Issues/registry.jsonl](Issues/registry.jsonl).
5. Для входных материалов использовать [Inbox/README.md](Inbox/README.md).
6. Для пользовательских концепций начинать с [Concepts/README.md](Concepts/README.md).

Команды `старт`, `пинг` и `1` должны загружать минимальный контекст: этот файл, file index, link graph и relevant state. Полный startup protocol будет закреплён в `Protocols/common/startup.md`.

## State map

Верхний state ещё не создан. Зарезервированные primary sources:

| Путь | Назначение | План |
|---|---|---|
| `State/service_state.json` | Текущее состояние `Service Mode`. | Phase 2 |
| `State/execution_index_state.json` | Индекс состояния `Execution Mode` и списка концепций. | Phase 2 |
| `State/state_schema.md` | Схема state-файлов верхнего уровня, concept и issue. | Phase 2 |

## Протоколы

| Путь | Роль | План |
|---|---|---|
| `Protocols/common/startup.md` | Запуск режима и первичная загрузка state. | Phase 3 |
| `Protocols/common/context_loading.md` | Правила локального focus packet и выбора контекста. | Phase 3 |
| `Protocols/common/state_update.md` | Правила сохранения state до ответа пользователю. | Phase 3 |
| `Protocols/service/service_mode.md` | Границы и workflow `Service Mode`. | Phase 4 |
| `Protocols/execution/execution_mode.md` | Границы и workflow `Execution Mode`. | Phase 5 |

Пока эти файлы не созданы, данный раздел является навигационной картой плановых primary sources, а не заменой протоколов.

## Issues

Service-level issue регистрируются в [Issues/registry.jsonl](Issues/registry.jsonl).
Папка `Issues/active/<issue_id>/` создаётся только при появлении первого active issue.
Каждая строка registry должна вести к конкретному `state.json`, `reason.md`, requirements, plan, solution, contract и output, когда эти файлы применимы.

## Concepts

Область пользовательских концепций описана в [Concepts/README.md](Concepts/README.md).
Каждая концепция будет жить в `Concepts/<concept_slug>/` и иметь собственные `README.md`, `manifest.jsonl`, `structure.md` и локальный registry issue.
Папка конкретной концепции создаётся только после появления реальной концепции.

## Repository map

| Файл | Назначение |
|---|---|
| [Repository/file_index.jsonl](Repository/file_index.jsonl) | Машинный индекс рабочих файлов репозитория. |
| [Repository/link_graph.md](Repository/link_graph.md) | Читаемая карта ссылок и первая orphan-проверка. |

## Правила навигации

- Каждый Markdown-файл начинается с заголовка первого уровня и блока `Назначение`.
- Каждый Markdown-файл, кроме корневого `README.md`, содержит ссылку возврата к parent-файлу.
- Подробный протокол живёт в своём primary source, а не копируется в соседние файлы.
- Новые файлы создаются только после lean gate: функция, источник истины, parent, достижимость и риск дублей.

## Проверка целостности

Файл считается рабочим только если он описан в `Repository/file_index.jsonl` или в локальном manifest и достижим от точки входа.
В Phase 1 активными считаются только файлы, уже физически созданные в репозитории.
Плановые пути без созданного файла указаны как code path, а не как Markdown-ссылка, чтобы не создавать broken links.
