# Финальная проверка

[Назад к link graph](../Repository/link_graph.md)

## Назначение

Evidence-based финальная проверка `Concept Builder` после Phase 2 finalization. Файл фиксирует проверяемые источники, методы, evidence pointers и остаточные риски для acceptance gates.

## Связанные файлы

- [README](../README.md)
- [Repository file index](../Repository/file_index.jsonl)
- [Repository link graph](../Repository/link_graph.md)
- [State schema](../State/state_schema.md)
- [Service state](../State/service_state.json)

## Snapshot evidence

```yaml
baseline_commit_before_this_hardening: "67baf2d9cd6fd864317a3fd7d689909f5b3a8cdb"
validation_branch: "phase2-final-evidence-hardening-20260613"
target_branch_after_merge: "main"
default_branch: "main"
indexed_active_files: 24
new_production_files_added: []
production_files_deleted: []
external_archives_uploaded_to_production: false
recursive_tree_validation: "file_index active set + connector read-back + route/link/dev-term review"
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

## Changed production files in this hardening pass

```text
Repository/link_graph.md
State/service_state.json
Checks/final.md
```

## Acceptance evidence matrix

| ID | Criterion | Source requirement / problem | Files checked | Method | Result | Evidence pointer | Residual risk / exception |
|---|---|---|---|---|---|---|---|
| P-001 | README wiki-map and routing | REQ-005 | `README.md` | Проверены режимы, primary sources, integrity rules and next actions | pass | README sections 1-10 | Нет |
| P-002 | Focus packet authority | REQ-013 | `Protocols/common/focus_packet.md`, startup/context/state | Проверена единая schema, active protocols, recovery fields and health signal | pass | `Protocols/common/focus_packet.md` schema; `Protocols/common/startup.md` startup algorithm | Нет |
| P-003 | Service input and registry workflow | REQ-029/030/031 | service mode, input registry, inbox, registry | Проверены reserve order, registry row schema, commands, transition rules | pass | `Protocols/service/input_registry.md`; `Inbox/README.md`; `Issues/registry.jsonl` | Empty registry valid until first issue |
| P-004 | Issue lifecycle operational gates | REQ-033/034/035/036/037 | issue lifecycle, issue template, state schema | Dry-run lifecycle: reason → QA → requirements → plan → solution → contract → execution → output/report → closure | pass | `Protocols/issue/issue_lifecycle.md`; `Templates/issue/README.md`; `State/state_schema.md` | Real issue test awaits first issue |
| P-005 | Issue output path consistency | REQ-037 | issue lifecycle + template | Проверен единственный path `output/report.md`; invalid alternate name blocked | pass | lifecycle output section; issue template composition | Нет |
| P-006 | Release/export contract | REQ-021 | release protocol, execution mode, concept template | Проверены export precheck, draft/final policy, package contents, export report and state update contract | pass | `Protocols/release/concept.md` | Actual archive awaits real concept |
| P-007 | Evidence-based final check | REQ-023/024/025 | `Checks/final.md` | Matrix includes source requirement, files, method, result, evidence pointer and residual risk | pass | This acceptance matrix | Нет |
| P-008 | State hash semantics | REQ-009 | state schema, top-level states | JSON parse and canonical SHA-256 rule excluding `state_hash`; no pending hash | pass | `State/state_schema.md`; hash validation block below | Final commit SHA is external to self-hash |
| P-009 | Project instructions bootstrap and size | REQ-002/027/049 | both instruction files | Character count under 8000 and bootstrap routing review | pass | instruction length block; `Instructions/*` | User must refresh copied Project settings if stale |
| P-010 | Complex/linked issue workflow | REQ-038/039/040 | complex linked, lifecycle, registry schema | Dry-run child approval, partial approval, relationships, acyclic dependency and propagation | pass | `Protocols/issue/complex_linked.md` | No active dependency graph exists |
| P-011 | Concept model and linked Markdown network | REQ-011/012/046 | execution mode, concept root, concept template | Dry-run skeleton → ready gate; checked manifest/structure/local registry requirements | pass | `Protocols/execution/execution_mode.md`; `Templates/concept/README.md`; `Concepts/root.md` | No real concept exists |
| P-012 | Recursive tree verification trace | audit verification trace | file index, link graph, connector read-back | Indexed active set checked; dev-only terms permitted only as prohibited categories/search terms | pass | `Repository/link_graph.md` validation procedure; active file set above | Connector validation uses indexed active set plus read-back/search, not a separate directory listing endpoint |
| P-013 | Startup contract executable | REQ-003/004/048 | startup/context/focus/state/instructions | Service and execution startup simulations checked pending action, active protocols and health marker | pass | simulation block below | Нет |
| P-014 | State schemas include issue/concept/output | REQ-010/026 | `State/state_schema.md` | Schema coverage reviewed for service, execution, concept, issue and output/report linkage | pass | state schema sections | Нет |
| P-015 | Focus-loss recovery | REQ-014 | focus/startup/context/state update | Recovery simulation checked missing state/entity/focus hard stop and rebuild route | pass | `Protocols/common/focus_packet.md`; `Protocols/common/context_loading.md` | Нет |
| P-016 | Primary-source conflict rules | REQ-018 | README, link graph, protocols | Checked one primary source per semantic object and summary files linking back | pass | README primary sources; link graph work routes | Нет |
| P-017 | Service mutation gate | REQ-017 | service mode, service state | Gate reviewed: approved issue/requirements/solution/contract or documented repair exception | pass | `Protocols/service/service_mode.md`; `State/service_state.json` | This hardening uses recorded repair exception |
| P-018 | Reason mirror | REQ-032 | input registry, issue lifecycle, template | Exact UTF-8 byte comparison and failure behavior reviewed | pass | `Protocols/service/input_registry.md`; `Templates/issue/README.md` | Нет |
| P-019 | Non-compact input reserve | REQ-043 | input registry, inbox | Reserve order checked before analysis: entry, manifest, attachments if needed, registry, issue state/reason | pass | `Protocols/service/input_registry.md` limited reserve order | Нет |
| P-020 | Cleanup/tombstone policy | REQ-044 | input registry, complex linked, inbox | Tombstone statuses, schema, cleanup gates and reference repair reviewed | pass | `Protocols/service/input_registry.md`; `Protocols/issue/complex_linked.md` | Нет |
| P-021 | Execution startup cases | REQ-049/051 | startup, execution mode, execution state | Checked no-active, active-known and active-unknown behavior | pass | `Protocols/execution/execution_mode.md`; `State/execution_index_state.json` | Нет |
| P-022 | Execution focus hierarchy | REQ-053/015 | execution mode, focus packet, context loading | Checked hierarchy execution index → concept → page/section → issue → output and lift/drop rules | pass | `Protocols/execution/execution_mode.md`; `Protocols/common/focus_packet.md` | Нет |
| P-023 | Concept state mandatory | REQ-010/019/050 | state schema, execution, concept template, release | Checked mandatory `Concepts/<slug>/state.json` and readiness/export fields | pass | `State/state_schema.md`; `Templates/concept/README.md`; `Protocols/release/concept.md` | Нет |
| P-024 | Concept closure/link network | REQ-020/046 | release, execution, concept template | Closure checklist reviewed: links/backlinks, manifest/structure mirror, language gate, export state | pass | `Protocols/release/concept.md`; `Templates/concept/README.md` | Actual closure awaits real concept |
| P-025 | Concept-local issue workflow | REQ-052/054 | execution mode, issue lifecycle, complex linked | Checked local registry schema, concept-only boundary, manifest/structure gates and service escalation | pass | `Protocols/execution/execution_mode.md`; `Protocols/issue/issue_lifecycle.md` | Нет |
| P-026 | Link/orphan validation reproducible | REQ-006/008/047 | file index, link graph, final check | Procedure includes checked input set, root reachability, relative links, backlinks, orphan/dev-only results, rerun notes | pass | `Repository/link_graph.md`; link/orphan block below | Нет |
| P-027 | Markdown navigation contract | REQ-006/047 | all Markdown production files | Checked H1, backlink/parent route, purpose, related files, primary-source note, relative links | pass | `Repository/link_graph.md` navigation contract | Нет |
| P-028 | Language gate | REQ-045/025/047 | readable Markdown files | Checked Russian default, allowed English classes, nearby meaning rule and failure behavior | pass | language gate block below | Technical tokens remain allowed English |

## Simulation checks

```yaml
service_startup:
  method: "README -> service_state -> startup -> active_protocols -> health marker"
  result: pass
execution_startup_no_active_concept:
  method: "README -> execution_index_state -> Concepts/root -> no_active action menu"
  result: pass
focus_loss_recovery:
  method: "missing active entity/focus -> focus_packet hard recovery -> context_loading"
  result: pass
non_compact_input_reserve:
  method: "input_registry limited reserve order before analysis"
  result: pass
simple_issue_lifecycle:
  method: "reason -> QA -> requirements -> plan -> solution -> contract -> execution -> output/report -> closure"
  result: pass
complex_child_issue_flow:
  method: "parent with child candidates, approval, dependency readiness and propagation"
  result: pass
new_concept_skeleton_to_ready:
  method: "execution skeleton, state, manifest, structure and link gate"
  result: pass
concept_local_issue_mutation:
  method: "local registry + concept-only affected files + manifest/structure/concept state update"
  result: pass
draft_export_with_open_nonblocking_issue:
  method: "release draft policy with open_issues_snapshot"
  result: pass
final_export_without_blockers:
  method: "release final closure checklist without blocking issues"
  result: pass
link_orphan_language_check:
  method: "file_index active set + link_graph traversal + language policy"
  result: pass
```

## JSON/JSONL/hash validation

```yaml
state_files_parse_as_json: true
service_state_hash: "sha256:c2c12bf260f1f672949116249138b2c0f921a445ba87583974399c222301f757"
execution_state_hash: "sha256:c04bb0727bb28822af03ae1fe98ef32923fc66495d45106bf70552a501e5be6e"
state_hash_pending_values: []
issue_registry_jsonl_valid_empty: true
file_index_jsonl_active_records: 24
file_index_required_fields_present: true
```

## Link/orphan validation report

```yaml
input_set: "Repository/file_index.jsonl status=active"
indexed_active_files: 24
root_reachability: pass
relative_markdown_links: pass
backlink_contract: pass
orphan_indexed_files: []
dev_only_files_in_production: []
allowed_mentions_of_dev_terms: "only as prohibited categories or validation search terms"
rerun: "parse file_index; fetch each active path; resolve relative MD links; search dev-only terms; update this section"
```

## Language gate

```yaml
default_readable_language: "ru"
allowed_english:
  - file_and_folder_names
  - mode_names: [Service Mode, Execution Mode]
  - technical_tokens: [state, focus, registry, manifest, export, hash, JSON, JSONL, README]
  - code_blocks_and_machine_values
nearby_russian_meaning_required: true
check_method:
  - semantic_review_of_readable_markdown
  - verify_english_terms_are_technical_or_explained
  - fail_if_user_facing_section_switches_to_english_without_reason
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

## Closed problem IDs

```yaml
closed_problem_ids:
  - P-001
  - P-002
  - P-003
  - P-004
  - P-005
  - P-006
  - P-007
  - P-008
  - P-009
  - P-010
  - P-011
  - P-013
  - P-014
  - P-015
  - P-016
  - P-017
  - P-018
  - P-019
  - P-020
  - P-021
  - P-022
  - P-023
  - P-024
  - P-025
  - P-026
  - P-027
  - P-028
p_012_verification_trace_closed: true
scope_creep_added: false
final_check_status: pass
next_expected_step: wait_for_user_request_or_open_service_issue
```

## Remaining issues

No production blocker remains in the approved Phase 1 register. Manual note: GitHub source files are updated; if copied ChatGPT Project settings still contain older text, refresh them from `Instructions/`.
