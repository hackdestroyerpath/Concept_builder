# Repository link graph

[Назад к README](../README.md)

## Назначение

Читаемая карта рабочих файлов `Concept Builder` и текущая orphan-проверка.

## Активные файлы

- [README](../README.md)
- [File index](file_index.jsonl)
- [Service instruction](../Instructions/concept_builder_service_mode_project_instruction.md)
- [Execution instruction](../Instructions/concept_builder_project_instruction.md)
- [Service state](../State/service_state.json)
- [Execution state](../State/execution_index_state.json)
- [State schema](../State/state_schema.md)
- [Startup protocol](../Protocols/common/startup.md)
- [Focus packet](../Protocols/common/focus_packet.md)
- [State update](../Protocols/common/state_update.md)
- [Issues registry](../Issues/registry.jsonl)
- [Inbox](../Inbox/README.md)
- [Concepts](../Concepts/root.md)

## Плановые файлы

- `Protocols/service/service_mode.md`
- `Protocols/execution/execution_mode.md`

## Проверка

```yaml
phase: 3
active_files_indexed: true
active_links_present: true
startup_protocol_created: true
focus_packet_created: true
state_update_created: true
context_loading_role: covered_by_focus_packet
status: pass_with_deprecated_alias_note
notes:
  - Protocols/common/focus_packet.md является текущим primary source для локального пакета контекста.
  - Instructions/concept_builder_execution_mode_project_instruction.md физически существует как лишний alias; canonical source: Instructions/concept_builder_project_instruction.md.
```
