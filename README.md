# Concept Builder

## 1. Назначение

`Concept Builder` — рабочий репозиторий для двух режимов: `Service Mode` обслуживает систему, `Execution Mode` ведёт пользовательские концепции. Этот файл является короткой картой запуска и отправляет к основным источникам правил.

Технические слова в обратных кавычках являются машинными именами, путями, режимами или устойчивыми терминами репозитория. Их смысл рядом описан по-русски, чтобы язык снова не превратился в кашу из двух алфавитов.

## 2. Быстрый старт

1. Открой этот файл.
2. Выбери режим по таблице ниже.
3. Открой верхнее состояние: [service_state.json](State/service_state.json) или [execution_index_state.json](State/execution_index_state.json).
4. Открой [file_index.jsonl](Repository/file_index.jsonl) и [link_graph.md](Repository/link_graph.md).
5. Выполни [протокол запуска](Protocols/common/startup.md): проверь `pending_user_action`, собери пакет фокуса и загрузи только пути из `active_protocols`.
6. Перед ответом сохраняй изменённые рабочие файлы и нужное состояние через GitHub Connector. `persisted=yes` допустимо только после фактической записи.

## 3. Маршрутизация режимов

| Ситуация | Режим | Основной источник |
|---|---|---|
| Правка протоколов, схем состояния, карт репозитория, инструкций проекта или служебных задач | `Service Mode` | [Протокол Service Mode](Protocols/service/service_mode.md) |
| Создание или развитие пользовательской концепции | `Execution Mode` | [Протокол Execution Mode](Protocols/execution/execution_mode.md) |
| Входные материалы, резерв входа, служебный реестр и зеркалирование причины | `Service Mode` | [Входные материалы и registry](Protocols/service/input_registry.md) |
| Жизненный цикл служебной или концептной задачи | общий процесс `issue` | [Жизненный цикл issue](Protocols/issue/issue_lifecycle.md) |
| Сложная, дочерняя или связанная задача | общий связанный процесс | [Сложные и связанные issue](Protocols/issue/complex_linked.md) |
| Черновой или финальный выпуск концепции | `Execution Mode` | [Выпуск концепции](Protocols/release/concept.md) |

## 4. Карта рабочих зон

- [Инструкции](Instructions/concept_builder_project_instruction.md) — короткие загрузчики режимов; полные правила живут в протоколах.
- [Состояние](State/state_schema.md) — схемы `state`, то есть файлов состояния.
- [Протоколы](Protocols/common/startup.md) — правила запуска, контекста, состояния, service, execution, issue и export.
- [Issues](Issues/registry.jsonl) — служебный реестр задач; рабочие папки создаются только по реальной задаче.
- [Inbox](Inbox/README.md) — входные материалы, резерв и очистка.
- [Concepts](Concepts/root.md) — пользовательские концепции; демонстрационные папки запрещены.
- [Templates](Templates/issue/README.md) — шаблоны будущих папок, не рабочие экземпляры.
- [Repository](Repository/link_graph.md) — индекс файлов, карта связей и договор навигации.
- [Checks](Checks/final.md) — финальная проверка и evidence, то есть доказательства.

## 5. Что читать сначала

Обязательный минимум: [file_index.jsonl](Repository/file_index.jsonl), [link_graph.md](Repository/link_graph.md), нужное состояние и протоколы из `active_protocols`. Остальные файлы читаются только если входят в пакет фокуса или нужны для проверки.

## 6. Основные источники

- Пакет фокуса: [focus_packet.md](Protocols/common/focus_packet.md).
- Схема состояния и хэш: [state_schema.md](State/state_schema.md).
- Запуск и восстановление: [startup.md](Protocols/common/startup.md).
- Загрузка контекста и экономная проверка: [context_loading.md](Protocols/common/context_loading.md).
- Сохранение состояния: [state_update.md](Protocols/common/state_update.md).
- Входы и реестр: [input_registry.md](Protocols/service/input_registry.md).
- Жизненный цикл задач: [issue_lifecycle.md](Protocols/issue/issue_lifecycle.md).
- Сложные связи задач: [complex_linked.md](Protocols/issue/complex_linked.md).
- Модель концепции: [execution_mode.md](Protocols/execution/execution_mode.md).
- Выпуск концепции: [concept.md](Protocols/release/concept.md).

## 7. Состояние и восстановление

Файлы `state` являются источником восстановления нового чата. Если `state_hash` неверен, `current_entity_id` потерян, активный state-файл не открывается, реестр конфликтует с состоянием или `context_confidence=low`, обычная работа блокируется до восстановления по [focus_packet.md](Protocols/common/focus_packet.md) и [context_loading.md](Protocols/common/context_loading.md).

## 8. Маршруты issue и concept

Служебная задача начинается с входа или реестра и проходит: вопросы, требования, план, решение, договор, выполнение, отчёт и закрытие. Задача концепции использует тот же путь, но меняет только файлы внутри `Concepts/<slug>/`, кроме разрешённого обновления верхнего состояния execution. Системный дефект, найденный в `Execution Mode`, переводится в служебную задачу.

## 9. Целостность и навигация

- `Repository/file_index.jsonl` перечисляет каждый активный рабочий файл.
- `Repository/link_graph.md` описывает достижимость от корня, обратные ссылки, договор навигации, проверку ссылок, сиротских файлов и языка.
- Один смысловой объект имеет один основной источник; остальные файлы дают краткую сводку и ссылку.
- Читаемые Markdown-файлы пишутся по-русски. Английский допустим только для имён файлов, путей, режимов, машинных значений и необходимых технических токенов с русским смыслом рядом.
- Новый рабочий файл создаётся только после экономной проверки из [context_loading.md](Protocols/common/context_loading.md).
- Handoff, audit, task-state archives, implementation reports и temporary notes не загружаются в рабочее дерево репозитория.

## 10. Следующие действия

Если пользователь просит обслужить систему — стартуй через `Service Mode`. Если пользователь просит создать или вести концепцию — стартуй через `Execution Mode`. Если фокус потерян или состояние конфликтует с реестром, сначала выполняется восстановление.
