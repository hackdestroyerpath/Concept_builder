# Repository link graph

[Назад к README](../README.md)

## Назначение

Карта достижимости active production-файлов, Markdown navigation contract и воспроизводимая процедура link/orphan/language validation. Машинный список путей хранится в [file_index.jsonl](file_index.jsonl); этот файл объясняет, как этот список проверять.

## Связанные файлы

- [README](../README.md)
- [Repository file index](file_index.jsonl)
- [Final check](../Checks/final.md)

## Active production tree

```text
README.md
Checks/final.md
Concepts/root.md
Inbox/README.md
Instructions/concept_builder_project_instruction.md
Instructions/concept_builder_service_mode_project_instruction.md
Issues/registry.jsonl
Protocols/common/context_loading.md
Protocols/common/focus_packet.md
Protocols/common/startup.md
Protocols/common/state_update.md
Protocols/execution/execution_mode.md
Protocols/issue/complex_linked.md
Protocols/issue/issue_lifecycle.md
Protocols/release/concept.md
Protocols/service/input_registry.md
Protocols/service/service_mode.md
Repository/file_index.jsonl
Repository/link_graph.md
State/execution_index_state.json
State/service_state.json
State/state_schema.md
Templates/concept/README.md
Templates/issue/README.md
```

Все эти пути должны открываться через GitHub Connector на целевой ветке. Папки `Issues/active/`, `Inbox/<input_id>/` и `Concepts/<slug>/` создаются только по реальным запросам и после обновления соответствующего registry/manifest.

## Root route

- [README](../README.md)
  - [Repository file index](file_index.jsonl)
  - [Final check](../Checks/final.md)
  - [State schema](../State/state_schema.md)
  - [Startup protocol](../Protocols/common/startup.md)
  - [Service Mode](../Protocols/service/service_mode.md)
  - [Execution Mode](../Protocols/execution/execution_mode.md)

## Work routes

| Route | Files |
|---|---|
| State | [service](../State/service_state.json), [execution](../State/execution_index_state.json), [schema](../State/state_schema.md) |
| Instructions | [execution](../Instructions/concept_builder_project_instruction.md), [service](../Instructions/concept_builder_service_mode_project_instruction.md) |
| Common protocols | [startup](../Protocols/common/startup.md), [context](../Protocols/common/context_loading.md), [focus](../Protocols/common/focus_packet.md), [state update](../Protocols/common/state_update.md) |
| Service | [mode](../Protocols/service/service_mode.md), [input registry](../Protocols/service/input_registry.md), [issue registry](../Issues/registry.jsonl), [inbox](../Inbox/README.md) |
| Execution | [mode](../Protocols/execution/execution_mode.md), [concepts](../Concepts/root.md), [release](../Protocols/release/concept.md), [concept template](../Templates/concept/README.md) |
| Issues | [lifecycle](../Protocols/issue/issue_lifecycle.md), [complex linked](../Protocols/issue/complex_linked.md), [issue template](../Templates/issue/README.md) |
| Checks | [final evidence](../Checks/final.md) |

## Markdown navigation contract

Каждый active Markdown-файл, кроме root README, обязан иметь:

1. один H1;
2. backlink к parent или root route;
3. раздел `Назначение`;
4. раздел `Связанные файлы`, если есть локальные зависимости;
5. primary-source note или ссылку на primary source;
6. только относительные repository links;
7. failure behavior, если файл задаёт gate или workflow.

Root README является исключением: он вместо backlink содержит compact wiki-map, routing, integrity rules и next actions.

## Backlink expectations

- Любой файл, указанный в `parent`, должен иметь route из README или work routes.
- Summary/template files ссылаются на primary source, но не вводят альтернативную схему.
- Issue/concept экземпляры обязаны иметь локальный backlink к parent concept/issue и строку в registry/manifest.
- `output/report.md` связан с issue state и registry row; имя `output_report.md` запрещено.

## Link/orphan validation procedure

1. Распарсить [file_index.jsonl](file_index.jsonl) как JSONL; каждая строка должна иметь `path`, `kind`, `owner_mode`, `purpose`, `parent`, `primary_source`, `described_in`, `status`.
2. Открыть каждый `status=active` path через GitHub Connector на проверяемой ветке.
3. Для каждого Markdown-файла извлечь относительные links `(...md)`, `(...json)`, `(...jsonl)` и проверить, что целевой путь существует в indexed active paths или является допустимым future-template path внутри issue/concept instance.
4. Проверить root reachability: README → route → file. Файл без route считается orphan даже если физически существует.
5. Проверить backlinks: child/summary files должны возвращаться к parent или primary source.
6. Проверить dev-only terms: `handoff`, `phase1_audit`, `task-state`, `implementation_report`, `checkpoint`, `temporary_notes`, `original_handoff`. Совпадения допустимы только в запретительных правилах README/link/final, не как production path.
7. Проверить language gate: readable Markdown по умолчанию русский; allowed English ограничен technical names/tokens.
8. Результат записать в [Checks/final.md](../Checks/final.md) с input set, method, result, exceptions and rerun notes.

## Validation evidence snapshot

```yaml
baseline_commit_before_this_hardening: "67baf2d9cd6fd864317a3fd7d689909f5b3a8cdb"
validation_branch: "phase2-final-evidence-hardening-20260613"
target_branch_after_merge: "main"
indexed_active_files: 24
new_production_files_added_by_this_hardening: []
production_files_deleted_by_this_hardening: []
dev_only_files_expected_in_production: []
recursive_tree_validation_method:
  - file_index_jsonl_parse
  - connector_fetch_file_for_each_indexed_path
  - root_route_and_backlink_review
  - dev_only_keyword_search
  - relative_link_resolution
final_evidence_file: "Checks/final.md"
```
