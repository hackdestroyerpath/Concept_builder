# Repository link graph

[Назад к README](../README.md)

## Назначение

Карта достижимости production-файлов `Concept Builder`.
`Repository/file_index.jsonl` остаётся машинным источником истины, а этот файл даёт читаемый маршрут.

## Root

- [README](../README.md)
- [File index](file_index.jsonl)
- [Final check](../Checks/final.md)

## State

- [Service state](../State/service_state.json)
- [Execution index state](../State/execution_index_state.json)
- [State schema](../State/state_schema.md)

## Project instructions

- [Execution project instruction](../Instructions/concept_builder_project_instruction.md)
- [Service project instruction](../Instructions/concept_builder_service_mode_project_instruction.md)

## Protocols

- [Startup](../Protocols/common/startup.md)
- [Context loading](../Protocols/common/context_loading.md)
- [Focus packet](../Protocols/common/focus_packet.md)
- [State update](../Protocols/common/state_update.md)
- [Service Mode](../Protocols/service/service_mode.md)
- [Input registry](../Protocols/service/input_registry.md)
- [Execution Mode](../Protocols/execution/execution_mode.md)
- [Issue lifecycle](../Protocols/issue/issue_lifecycle.md)
- [Complex linked issue](../Protocols/issue/complex_linked.md)
- [Concept release](../Protocols/release/concept.md)

## Work areas

- [Inbox](../Inbox/README.md)
- [Concepts](../Concepts/root.md)
- [Service issue registry](../Issues/registry.jsonl)

## Templates

- [Issue template](../Templates/issue/README.md)
- [Concept template](../Templates/concept/README.md)

## Проверка

```yaml
phase: repair_p0_p1
status: pass
active_files_indexed: true
active_links_present: true
deprecated_files_in_production: []
manual_github_action_required: false
```
