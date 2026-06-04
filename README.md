# Concept Builder

## Назначение

`Concept Builder` это GitHub-центрированная система для создания, уточнения, проверки и экспорта больших агентно-ориентированных концепций. Репозиторий хранит состояние, протоколы, реестры, concept-файлы и навигацию так, чтобы новый чат мог продолжить работу с последней устойчивой точки, а не устраивать раскопки в переписке, как будто это археологический грант.

Система не является монолитным промптом. Project instructions в папке [`Instructions/`](Instructions/README.md) служат короткими загрузчиками, а полный рабочий контекст хранится в файлах репозитория.

## Режимы работы

| Режим | Объект работы | Основная страница |
|---|---|---|
| `Service Mode` | Сам `Concept Builder`: инструкции, state, протоколы, service issue, индексы и проверки | [`Protocols/service/service_mode.md`](Protocols/service/service_mode.md) |
| `Execution Mode` | Пользовательские концепции внутри `Concepts/` | [`Protocols/execution/execution_mode.md`](Protocols/execution/execution_mode.md) |

## Быстрый старт агента

1. Открой этот `README.md`.
2. Определи режим проекта ChatGPT: `Service Mode` или `Execution Mode`.
3. Прочитай соответствующий state:
   - [`State/service_state.json`](State/service_state.json) для `Service Mode`;
   - [`State/execution_index_state.json`](State/execution_index_state.json) для `Execution Mode`.
4. Прочитай только протоколы, перечисленные в `active_protocols`.
5. Восстанови фокус, отложенное действие и следующий шаг.
6. Ответь пользователю по шаблону запуска из [`Protocols/common/startup.md`](Protocols/common/startup.md).

## Карта state

| Файл | Назначение |
|---|---|
| [`State/state_schema.md`](State/state_schema.md) | Схема state-файлов и правила восстановления |
| [`State/service_state.json`](State/service_state.json) | Верхнее состояние обслуживания системы |
| [`State/execution_index_state.json`](State/execution_index_state.json) | Индекс пользовательских концепций и активного фокуса |

## Протоколы

| Область | Файл | Назначение |
|---|---|---|
| общий | [`Protocols/common/startup.md`](Protocols/common/startup.md) | Запуск режима и первичная загрузка состояния |
| общий | [`Protocols/common/context_loading.md`](Protocols/common/context_loading.md) | Локальный набор контекста и стек фокуса |
| общий | [`Protocols/common/state_update.md`](Protocols/common/state_update.md) | Сохранение state, registry и обязательных артефактов |
| общий | [`Protocols/common/issue_lifecycle.md`](Protocols/common/issue_lifecycle.md) | Жизненный цикл issue от входных данных до output |
| общий | [`Protocols/common/linked_issues.md`](Protocols/common/linked_issues.md) | Complex issue, связи, порядок выполнения и очистка |
| сервис | [`Protocols/service/service_mode.md`](Protocols/service/service_mode.md) | Правила обслуживания самой системы |
| исполнительный | [`Protocols/execution/execution_mode.md`](Protocols/execution/execution_mode.md) | Правила работы с пользовательскими концепциями |
| исполнительный | [`Protocols/execution/concept_export.md`](Protocols/execution/concept_export.md) | Проверка и экспорт концепции |

## Service issue

Service issue хранятся в [`Issues/`](Issues/README.md). Машинный реестр: [`Issues/registry.jsonl`](Issues/registry.jsonl). Активные issue создаются в `Issues/active/<issue_id>/`.

Concept issue хранятся внутри конкретной концепции: `Concepts/<concept_slug>/Issues/`.

## Inbox и входные данные

Новые входные данные сохраняются в [`Inbox/`](Inbox/README.md) до ответа пользователю. Вложения не становятся точкой входа сами по себе. Если реестр ещё не создан, state должен явно показывать `input_saved_pending_analysis`.

## Concepts

Пользовательские концепции хранятся в [`Concepts/`](Concepts/README.md). Минимальный каркас концепции задан в [`Concepts/_template/`](Concepts/_template/README.md): `README.md`, `about.md`, `operating_model.md`, `requirements.md`, `process.md`, `manifest.jsonl`, `structure.md` и локальные `Issues/`.

## Карта репозитория

| Файл | Назначение |
|---|---|
| [`Repository/file_index.jsonl`](Repository/file_index.jsonl) | Машинный индекс всех рабочих файлов репозитория |
| [`Repository/link_graph.md`](Repository/link_graph.md) | Читаемая карта ссылок и результат проверки достижимости |

## Инварианты

- Каждый MD-файл, кроме этой точки входа, имеет ссылку возврата на parent.
- Подробные правила живут в primary source, а `README.md` даёт только краткое summary и ссылку.
- Агент движется сверху вниз: repository → concept → issue → requirements → solution → output.
- После закрытия локального фокуса агент обновляет parent state и поднимается по стеку фокуса.
- ТЗ-архив реализации, checkpoint-архивы и временные отчёты разработки в рабочий GitHub не входят.
