# Concept Builder

## Назначение

`Concept Builder` — рабочий репозиторий для двух режимов: обслуживания самой системы и ведения пользовательских концепций. Этот файл является короткой wiki-map: он маршрутизирует, но не дублирует протоколы.

## Как стартовать

1. Открой `README.md`.
2. Выбери режим: `Service Mode` (сервисный режим) или `Execution Mode` (исполнительный режим).
3. Открой нужный верхний state: [service_state.json](State/service_state.json) или [execution_index_state.json](State/execution_index_state.json).
4. Выполни [Startup protocol](Protocols/common/startup.md): проверь `pending_user_action`, собери focus packet и загрузи только `active_protocols`.
5. Перед ответом сохраняй изменённые production-файлы и relevant state через GitHub Connector. `persisted=yes` разрешён только после фактической записи.

## Routing режимов

| Ситуация | Режим | Primary source |
|---|---|---|
| Правка протоколов, state schema, repository map, project instructions, service issue | `Service Mode` | [Service Mode protocol](Protocols/service/service_mode.md) |
| Создание или развитие пользовательской концепции | `Execution Mode` | [Execution Mode protocol](Protocols/execution/execution_mode.md) |
| Lifecycle любой service/concept issue | общий issue workflow | [Issue lifecycle](Protocols/issue/issue_lifecycle.md) |
| Complex, child или linked issue | общий linked workflow | [Complex linked issue](Protocols/issue/complex_linked.md) |
| Выпуск или экспорт концепции | `Execution Mode` | [Concept release](Protocols/release/concept.md) |

## Основные зоны

- [Instructions](Instructions/concept_builder_project_instruction.md) — короткие project instructions; полные правила здесь не живут.
- [State](State/state_schema.md) — service, execution, concept, issue и output state.
- [Protocols](Protocols/common/startup.md) — исполняемые правила работы.
- [Issues](Issues/registry.jsonl) — service issue registry; active issue folders создаются только по реальной задаче.
- [Inbox](Inbox/README.md) — входные материалы и limited reserve.
- [Concepts](Concepts/root.md) — пользовательские концепции; demo folders запрещены.
- [Templates](Templates/issue/README.md) — шаблоны структуры, не рабочие экземпляры.
- [Repository](Repository/link_graph.md) — индекс, граф ссылок и validation contract.
- [Checks](Checks/final.md) — финальная evidence-based проверка.

## Что читать first

Обязательный минимум: [file_index.jsonl](Repository/file_index.jsonl), [link_graph.md](Repository/link_graph.md), relevant state и `active_protocols` из state. Остальные файлы читаются только если они входят в focus packet или нужны для проверки.

## Primary sources

- Focus packet: [Protocols/common/focus_packet.md](Protocols/common/focus_packet.md).
- State schema и hash: [State/state_schema.md](State/state_schema.md).
- Startup/recovery: [Protocols/common/startup.md](Protocols/common/startup.md).
- Input/registry/reason mirror: [Protocols/service/input_registry.md](Protocols/service/input_registry.md).
- Issue lifecycle: [Protocols/issue/issue_lifecycle.md](Protocols/issue/issue_lifecycle.md).
- Concept model и concept issue: [Protocols/execution/execution_mode.md](Protocols/execution/execution_mode.md).
- Export/closure: [Protocols/release/concept.md](Protocols/release/concept.md).

## Что не читать без причины

Не загружай implementation archives, checkpoint archives, temporary notes, все concepts сразу, все active issue сразу и attachments вне текущего focus. Если нужен дополнительный контекст, запиши `reload_reason` в focus packet.

## Integrity rules

- `Repository/file_index.jsonl` должен перечислять каждый active production-файл.
- `Repository/link_graph.md` должен описывать reachable route, Markdown navigation contract, проверку ссылок и исключения.
- Один смысловой объект имеет один primary source; остальные файлы дают summary и ссылку.
- Читаемые Markdown-файлы пишутся на русском. Английский допустим для имён файлов, режимов, сервисов и машинных токенов с русским смыслом рядом при необходимости.
- Новый production-файл создаётся только после lean gate из [context_loading.md](Protocols/common/context_loading.md).
- Временные handoff, audit, task-state и implementation reports не загружаются в production tree.

## Next actions

Если пользователь просит обслужить систему — стартуй через `Service Mode`. Если пользователь просит создать или вести концепцию — стартуй через `Execution Mode`. Если focus потерян или state конфликтует с registry, сначала выполняется recovery, а не героическое гадание по кофейной гуще.
