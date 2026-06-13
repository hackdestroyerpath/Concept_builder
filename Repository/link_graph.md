# Repository link graph

[Назад к README](../README.md)

## Назначение

Карта достижимости active production-файлов и локальный Markdown navigation contract. Машинный список путей хранится в [file_index.jsonl](file_index.jsonl).

## Связанные файлы

- [README](../README.md)
- [Repository file index](file_index.jsonl)
- [Final check](../Checks/final.md)

## Root route

- [README](../README.md)
  - [Repository file index](file_index.jsonl)
  - [Final check](../Checks/final.md)
  - [State schema](../State/state_schema.md)
  - [Startup protocol](../Protocols/common/startup.md)

## Work routes

- State: [service](../State/service_state.json), [execution](../State/execution_index_state.json), [schema](../State/state_schema.md)
- Instructions: [execution](../Instructions/concept_builder_project_instruction.md), [service](../Instructions/concept_builder_service_mode_project_instruction.md)
- Common protocols: [startup](../Protocols/common/startup.md), [context](../Protocols/common/context_loading.md), [focus](../Protocols/common/focus_packet.md), [state update](../Protocols/common/state_update.md)
- Service: [mode](../Protocols/service/service_mode.md), [input registry](../Protocols/service/input_registry.md), [issue registry](../Issues/registry.jsonl), [inbox](../Inbox/README.md)
- Execution: [mode](../Protocols/execution/execution_mode.md), [concepts](../Concepts/root.md), [release](../Protocols/release/concept.md)
- Issues: [lifecycle](../Protocols/issue/issue_lifecycle.md), [complex linked](../Protocols/issue/complex_linked.md), [template](../Templates/issue/README.md)
- Concepts: [template](../Templates/concept/README.md)

## Markdown navigation contract

Каждый active Markdown-файл, кроме root README, должен иметь один H1, ссылку назад к parent, раздел `Назначение`, раздел `Связанные файлы`, относительные links only, и primary-source note или ссылку на primary source. README является root exception и обязан содержать wiki-map, routing режимов, integrity rules и next actions.

## Link validation procedure

1. Сверить indexed paths из `Repository/file_index.jsonl` с GitHub tree.
2. Открыть каждый indexed path через GitHub Connector.
3. Проверить relative Markdown links.
4. Проверить root reachability и backlink map.
5. Проверить отсутствие лишних рабочих материалов вне production scope.
6. Записать результат в [Checks/final.md](../Checks/final.md).

## Validation evidence

```yaml
baseline_commit_sha: "2057c43b79d03c409079f17b136849615bf0ca51"
recursive_tree_checked: true
indexed_active_files: 24
broken_relative_links: []
orphan_indexed_files: []
final_evidence_file: Checks/final.md
```
