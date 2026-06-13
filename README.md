# Concept Builder

## 1. Назначение

`Concept Builder` — production-репозиторий для двух режимов работы: `Service Mode` обслуживает саму систему, `Execution Mode` ведёт пользовательские концепции. Этот README является короткой wiki-map: он маршрутизирует к primary sources, но не переписывает протоколы заново.

## 2. Быстрый старт

1. Открой этот файл.
2. Выбери режим по таблице ниже.
3. Открой верхний state: [service_state.json](State/service_state.json) или [execution_index_state.json](State/execution_index_state.json).
4. Открой [file_index.jsonl](Repository/file_index.jsonl) и [link_graph.md](Repository/link_graph.md).
5. Выполни [Startup protocol](Protocols/common/startup.md): проверь `pending_user_action`, собери focus packet и загрузи только `active_protocols`.
6. Перед ответом сохраняй изменённые production-файлы и relevant state через GitHub Connector. `persisted=yes` допустим только после фактической записи.

## 3. Routing режимов

| Ситуация | Режим | Primary source |
|---|---|---|
| Правка протоколов, state schema, repository maps, project instructions, service issue | `Service Mode` | [Service Mode protocol](Protocols/service/service_mode.md) |
| Создание или развитие пользовательской концепции | `Execution Mode` | [Execution Mode protocol](Protocols/execution/execution_mode.md) |
| Intake, input reserve, service registry, reason mirror | `Service Mode` | [Input registry](Protocols/service/input_registry.md) |
| Lifecycle service/concept issue | общий issue workflow | [Issue lifecycle](Protocols/issue/issue_lifecycle.md) |
| Complex, child или linked issue | общий linked workflow | [Complex linked issue](Protocols/issue/complex_linked.md) |
| Draft/final export концепции | `Execution Mode` | [Concept release](Protocols/release/concept.md) |

## 4. Карта рабочих зон

- [Instructions](Instructions/concept_builder_project_instruction.md) — короткие project instructions; полные правила живут в protocol files.
- [State](State/state_schema.md) — service, execution, concept, issue и output state.
- [Protocols](Protocols/common/startup.md) — исполняемые правила startup, context, state, service, execution, issue и release.
- [Issues](Issues/registry.jsonl) — service issue registry; active issue folders создаются только по реальной задаче.
- [Inbox](Inbox/README.md) — входные материалы, reserve order и cleanup.
- [Concepts](Concepts/root.md) — пользовательские концепции; demo folders запрещены.
- [Templates](Templates/issue/README.md) — шаблоны будущих issue/concept folders, не рабочие экземпляры.
- [Repository](Repository/link_graph.md) — file index, graph, navigation contract и validation procedure.
- [Checks](Checks/final.md) — evidence-based acceptance report.

## 5. Что читать first

Обязательный минимум: [file_index.jsonl](Repository/file_index.jsonl), [link_graph.md](Repository/link_graph.md), relevant state и `active_protocols` из state. Остальные файлы читаются только если они входят в focus packet или нужны для проверки.

## 6. Primary sources

- Focus packet: [Protocols/common/focus_packet.md](Protocols/common/focus_packet.md).
- State schema и hash: [State/state_schema.md](State/state_schema.md).
- Startup/recovery: [Protocols/common/startup.md](Protocols/common/startup.md).
- Context loading и lean gate: [Protocols/common/context_loading.md](Protocols/common/context_loading.md).
- State persistence: [Protocols/common/state_update.md](Protocols/common/state_update.md).
- Service/input/registry/reason mirror: [Protocols/service/input_registry.md](Protocols/service/input_registry.md).
- Issue lifecycle: [Protocols/issue/issue_lifecycle.md](Protocols/issue/issue_lifecycle.md).
- Complex/linked issue: [Protocols/issue/complex_linked.md](Protocols/issue/complex_linked.md).
- Concept model и concept issue: [Protocols/execution/execution_mode.md](Protocols/execution/execution_mode.md).
- Export/closure: [Protocols/release/concept.md](Protocols/release/concept.md).

## 7. State и recovery

State является источником восстановления нового чата. Если `state_hash` invalid, `current_entity_id` потерян, active state file отсутствует, registry конфликтует со state или `context_confidence=low`, обычная работа блокируется до recovery по [focus_packet.md](Protocols/common/focus_packet.md) и [context_loading.md](Protocols/common/context_loading.md).

## 8. Issue и concept routes

Service issue начинается с intake/registry и проходит `QA → requirements → plan → solution → contract → execution → output/report → closure`. Concept issue использует тот же lifecycle, но меняет только файлы внутри `Concepts/<slug>/`, кроме allowed update верхнего execution state. Системный дефект, найденный в `Execution Mode`, переводится в service issue.

## 9. Integrity и navigation rules

- `Repository/file_index.jsonl` перечисляет каждый active production-файл.
- `Repository/link_graph.md` описывает root reachability, backlinks, Markdown navigation contract, link/orphan validation и language gate.
- Один смысловой объект имеет один primary source; остальные файлы дают summary и ссылку.
- Читаемые Markdown-файлы пишутся на русском. Английский допустим для имён файлов, режимов, сервисов и машинных токенов; смысловые английские термины получают русский смысл рядом или входят в exception list.
- Новый production-файл создаётся только после lean gate из [context_loading.md](Protocols/common/context_loading.md).
- Handoff, audit, task-state archives, implementation reports и temporary notes не загружаются в production tree.

## 10. Next actions

Если пользователь просит обслужить систему — стартуй через `Service Mode`. Если пользователь просит создать или вести концепцию — стартуй через `Execution Mode`. Если focus потерян или state конфликтует с registry, сначала выполняется recovery.
