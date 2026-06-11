# Финальная проверка

[Назад к README](../README.md)

## Назначение

Файл фиксирует финальный контроль готовности `Concept Builder` после repair pass P0/P1.

## Проверки

```yaml
repository_entrypoint_exists: true
file_index_exists: true
link_graph_exists: true
service_state_loads: true
execution_state_loads: true
project_instructions_exist: true
core_protocols_exist: true
issue_lifecycle_exists: true
complex_linked_issue_exists: true
input_registry_exists: true
concept_release_exists: true
templates_exist: true
deprecated_files_removed: true
implementation_archives_in_production: false
checkpoint_archives_in_production: false
manual_github_action_required: false
```

## Active production scope

Production scope задаётся `Repository/file_index.jsonl`.
Все перечисленные active paths должны существовать и открываться через GitHub Connector.

## Deprecated cleanup

Удалены из production scope:

- `Repository/file_index_extra.jsonl`;
- `Repository/link_graph_phase7.md`;
- `Instructions/concept_builder_execution_mode_project_instruction.md`.

## Финальный статус

```yaml
final_check_status: pass
next_expected_step: wait_for_user_request_or_open_service_issue
```
