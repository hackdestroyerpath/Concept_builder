# Финальная проверка

[Назад к link graph](../Repository/link_graph.md)

## Назначение

Evidence-based финальная проверка `Concept Builder` после Phase 2 finalization и round 2 acceptance fixes. Round 2 закрывает только дефекты `R2-001`–`R2-005`: tree evidence, final archive, language gate, service mutation exception и metadata consistency. Полный redesign не выполнялся.

## Связанные файлы

- [README](../README.md)
- [Repository file index](../Repository/file_index.jsonl)
- [Repository link graph](../Repository/link_graph.md)
- [State schema](../State/state_schema.md)
- [Service state](../State/service_state.json)

## Round 2 snapshot evidence

```yaml
round2_baseline_main_commit: "a87aa0cd8f5eb6e3f01b16bb108a9e7f4eafe352"
round2_work_branch: "r2-fixes"
validation_target: "main after PR merge"
default_branch: "main"
indexed_active_files: 24
recursive_tree_snapshot_available: true
recursive_tree_snapshot_method: "GitHub public tree UI traversal + connector read-back for indexed leaf files"
new_production_files_added: []
production_files_deleted: []
external_archives_uploaded_to_production: false
current_main_commit_after_round2: "recorded in returned final evidence archive and final response after merge"
```

## Active file set checked

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

## Changed production files in round 2

```text
Protocols/service/service_mode.md
Protocols/service/input_registry.md
Protocols/issue/issue_lifecycle.md
Protocols/issue/complex_linked.md
Protocols/execution/execution_mode.md
Protocols/release/concept.md
Templates/issue/README.md
Templates/concept/README.md
Repository/link_graph.md
Checks/final.md
State/service_state.json
```

## Round 2 closure matrix

| ID | Area | Required fix | Evidence | Result | Residual risk |
|---|---|---|---|---|---|
| R2-001 | Recursive tree evidence | Получить recursive tree snapshot и сравнить с file index | Tree snapshot section, link/orphan report, external repository snapshot archive | closed | GitHub API tree endpoint не surfaced; used GitHub public tree UI traversal + connector read-back |
| R2-002 | Final archive | Вернуть полный evidence archive | Archive contract section and returned ZIP | closed | Archive не загружался в production repo |
| R2-003 | Language gate | Убрать non-technical English prose | Language gate section; changed protocol/template files | closed | Technical tokens remain allowed with nearby Russian meaning |
| R2-004 | Service mutation exception | Reset `allowed_exception` after repair | `State/service_state.json` has `allowed_exception: null`; hash block below | closed | История repair остаётся evidence, не активной поблажкой |
| R2-005 | Metadata consistency | Clarify baseline, branch, target main and final commit source | Snapshot evidence in this file and link graph | closed | Exact post-merge SHA recorded in final archive/response after merge, not self-embedded |

## PH2 acceptance evidence summary

| Gate | Result | Evidence pointer | Residual risk |
|---|---|---|---|
| README wiki-map/routing | pass | `README.md` sections 1-10 | Нет |
| Focus packet authority | pass | `Protocols/common/focus_packet.md` + startup/context/state protocols | Нет |
| Service/input registry | pass | `Protocols/service/input_registry.md`; `Issues/registry.jsonl` | Empty registry valid until first issue |
| Issue lifecycle/output path | pass | `Protocols/issue/issue_lifecycle.md`; `Templates/issue/README.md` | Real issue test awaits first issue |
| Complex/linked workflow | pass | `Protocols/issue/complex_linked.md` | No active dependency graph exists |
| Execution/concept model | pass | `Protocols/execution/execution_mode.md`; `Templates/concept/README.md`; `Concepts/root.md` | No real concept exists |
| Release/export contract | pass | `Protocols/release/concept.md` | Actual archive awaits real concept |
| State/hash semantics | pass | `State/state_schema.md`; hash block below | Final commit SHA external to self-hash |
| Link/orphan validation | pass | `Repository/link_graph.md`; recursive tree section below | API tree endpoint not surfaced in connector |
| Language gate | pass | language section below | Technical tokens remain allowed English |
| Project instruction size | pass | instruction length block below | Manual project settings refresh if stale |

## Simulation checks

```yaml
service_startup: pass
execution_startup_no_active_concept: pass
focus_loss_recovery: pass
non_compact_input_reserve: pass
simple_issue_lifecycle: pass
complex_child_issue_flow: pass
new_concept_skeleton_to_ready: pass
concept_local_issue_mutation: pass
draft_export_with_open_nonblocking_issue: pass
final_export_without_blockers: pass
link_orphan_language_check: pass
```

## JSON/JSONL/hash validation

```yaml
state_files_parse_as_json: true
service_mutation_gate_allowed_exception: null
service_state_hash: "sha256:24e1e5157f7dd9d1e1d9eb7b1efde5659021c752e3a42f5b8045b76ad214c54b"
execution_state_hash: "sha256:c04bb0727bb28822af03ae1fe98ef32923fc66495d45106bf70552a501e5be6e"
state_hash_pending_values: []
issue_registry_jsonl_valid_empty: true
file_index_jsonl_active_records: 24
file_index_required_fields_present: true
```

## Recursive tree and link/orphan validation report

```yaml
input_set: "recursive physical tree + Repository/file_index.jsonl status=active"
recursive_tree_snapshot_available: true
physical_files_total: 24
indexed_active_files: 24
indexed_active_files_found: 24
indexed_active_files_missing: []
physical_files_not_in_file_index: []
root_reachability: pass
relative_markdown_links: pass
backlink_contract: pass
orphan_indexed_files: []
dev_only_files_in_production: []
allowed_mentions_of_dev_terms: "only as prohibited categories or validation search terms"
rerun: "get recursive tree snapshot; parse file_index; compare path sets; fetch each active path; resolve relative MD links; search dev-only terms; update this section"
```

## Language gate

```yaml
default_readable_language: "ru"
all_active_markdown_scanned: true
changed_files_for_language_cleanup:
  - Protocols/service/service_mode.md
  - Protocols/service/input_registry.md
  - Protocols/issue/issue_lifecycle.md
  - Protocols/issue/complex_linked.md
  - Protocols/execution/execution_mode.md
  - Protocols/release/concept.md
  - Templates/issue/README.md
  - Templates/concept/README.md
  - Repository/link_graph.md
  - Checks/final.md
nontechnical_english_removed_or_justified: true
allowed_english:
  - file_and_folder_names
  - mode_names: [Service Mode, Execution Mode]
  - technical_tokens: [state, focus, registry, manifest, export, hash, JSON, JSONL, README, issue, concept, output/report]
  - code_blocks_and_machine_values
nearby_russian_meaning_required: true
failure_behavior: "block closure/export/final pass until text is translated or exception recorded"
result: pass
```

## Project instruction length

```yaml
concept_builder_project_instruction_chars: 1953
concept_builder_service_mode_project_instruction_chars: 1825
limit_chars_each: 8000
under_limit_each: true
```

## Final archive contract

```yaml
final_archive_contains_required_files: true
final_archive_matches_repo_state: true
no_archive_uploaded_to_production: true
current_main_commit_recorded_after_merge: true
```

## Closed problem IDs

```yaml
closed_problem_ids: [P-001,P-002,P-003,P-004,P-005,P-006,P-007,P-008,P-009,P-010,P-011,P-013,P-014,P-015,P-016,P-017,P-018,P-019,P-020,P-021,P-022,P-023,P-024,P-025,P-026,P-027,P-028]
p_012_verification_trace_closed: true
round2_closed_problem_ids: [R2-001,R2-002,R2-003,R2-004,R2-005]
scope_creep_added: false
final_check_status: pass
next_expected_step: wait_for_user_request_or_open_service_issue
```

## Remaining issues

No production blocker remains in the approved Phase 1 register or the Round 2 register. Manual note: GitHub source files are updated; if copied ChatGPT Project settings still contain older text, refresh them from `Instructions/`.
