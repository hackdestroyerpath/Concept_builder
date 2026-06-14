# Финальная проверка

[Назад к карте связей](../Repository/link_graph.md)

## Назначение

Финальная проверка после Phase 2, Round 2, Round 3, Round 4 и узкого Round 5 final acceptance repair. Round 5 закрывает только `R5-001`–`R5-003`: hash состояния service, разделение `scanned_files` и `changed_files`, а также замену противоречивого внешнего архива. Phase 2 и языковая чистка не выполнялись повторно.

## Связанные файлы

- [README](../README.md)
- [Индекс файлов](../Repository/file_index.jsonl)
- [Карта связей](../Repository/link_graph.md)
- [Схема состояния](../State/state_schema.md)
- [Состояние service](../State/service_state.json)

## Снимок доказательств Round 4 и Round 5

```yaml
round4_pr_number: 8
round4_pr_merged: true
round4_merge_commit: "ef2e6d9627c3cea0ebc9bdfae01fae72f975770e"
round4_baseline_main_commit: "db0bab94418165a1d9c9302d43977b3cee7d6b8b"
round4_work_branch: "agent/20260614-round4-language-gate"
round4_commits: 21
round4_changed_files_count: 14
round4_scanned_markdown_files_count: 20
round4_false_changed_files_count_before_r5: 7
round5_scope: [R5-001,R5-002,R5-003]
round5_production_files_expected: ["Checks/final.md","State/service_state.json"]
new_production_files_added_by_round5: []
production_files_deleted_by_round5: []
external_archives_uploaded_to_production: false
validation_target: "main после Round 5 PR merge"
default_branch: "main"
```

## Файлы, изменённые PR #8 в Round 4

```text
Checks/final.md
Inbox/README.md
Instructions/concept_builder_service_mode_project_instruction.md
Protocols/execution/execution_mode.md
Protocols/issue/complex_linked.md
Protocols/issue/issue_lifecycle.md
Protocols/release/concept.md
Protocols/service/input_registry.md
README.md
Repository/link_graph.md
State/service_state.json
State/state_schema.md
Templates/concept/README.md
Templates/issue/README.md
```

## Markdown-файлы, просканированные Round 4

Этот список описывает охват скана языка. Он не является списком изменённых файлов.

```text
README.md
Repository/link_graph.md
Inbox/README.md
Concepts/root.md
Instructions/concept_builder_project_instruction.md
Instructions/concept_builder_service_mode_project_instruction.md
State/state_schema.md
Protocols/common/startup.md
Protocols/common/context_loading.md
Protocols/common/focus_packet.md
Protocols/common/state_update.md
Protocols/service/service_mode.md
Protocols/service/input_registry.md
Protocols/execution/execution_mode.md
Protocols/issue/issue_lifecycle.md
Protocols/issue/complex_linked.md
Protocols/release/concept.md
Templates/issue/README.md
Templates/concept/README.md
Checks/final.md
```

## Файлы, ошибочно смешанные с changed-files evidence до Round 5

Эти файлы входили в скан Round 4, но не были изменены PR #8.

```text
Concepts/root.md
Instructions/concept_builder_project_instruction.md
Protocols/common/context_loading.md
Protocols/common/focus_packet.md
Protocols/common/startup.md
Protocols/common/state_update.md
Protocols/service/service_mode.md
```

## Round 4 closure matrix

| ID | Область | Требуемое исправление | Доказательство | Результат | Остаточный риск |
|---|---|---|---|---|---|
| R4-001 | Язык Markdown | Убрать обычную английскую прозу во всех активных читаемых Markdown-файлах | Скан 20 файлов; Round 4 PR #8 | закрыто | Английский оставлен только в разрешённых категориях |
| R4-002 | Evidence table | Зафиксировать остаточный английский по file/line/category | Round 4 evidence; список скана выше | закрыто | Нет |
| R4-003 | State/hash | Обновить `State/service_state.json`, оставить `allowed_exception: null`, пересчитать hash | Superseded by Round 5 validation ниже | закрыто после R5 | Нет |

## Round 5 closure matrix

| ID | Область | Требуемое исправление | Доказательство | Результат | Остаточный риск |
|---|---|---|---|---|---|
| R5-001 | `State/service_state.json` | Исправить hash и обновить состояние финального repair | `state_hash_valid: true`; reported и recomputed совпадают | закрыто | Нет |
| R5-002 | changed-files evidence | Разделить `scanned_files` и `changed_files`; не заявлять 21 changed files для PR #8 | PR #8 changed files count = 14; scanned Markdown count = 20; смешанные 7 файлов вынесены отдельно | закрыто | Нет |
| R5-003 | внешний архив | Заменить противоречивый архив новым финальным архивом вне production repo | Архив возвращается отдельно; production repo не содержит handoff/archive/evidence files | закрыто | Нет |

## JSON/JSONL/hash validation после Round 5

```yaml
state_files_parse_as_json: true
service_mutation_gate_allowed_exception: null
service_state_hash_reported: "sha256:ccba445698983427cbb5d1db29d23c3d94615f4c6130910a91faa9590b04af40"
service_state_hash_recomputed: "sha256:ccba445698983427cbb5d1db29d23c3d94615f4c6130910a91faa9590b04af40"
service_state_hash_valid: true
service_state_hash_rule: "sha256 over canonical JSON excluding state_hash; sort_keys=true; compact separators; UTF-8"
service_state_output_files: ["Checks/final.md","State/service_state.json"]
execution_state_hash: "sha256:c04bb0727bb28822af03ae1fe98ef32923fc66495d45106bf70552a501e5be6e"
state_hash_pending_values: []
issue_registry_jsonl_valid_empty: true
file_index_jsonl_active_records: 24
file_index_required_fields_present: true
```

## Языковой барьер Round 4

```yaml
default_readable_language: "ru"
all_active_markdown_scanned: true
scanned_files_count: 20
nontechnical_english_prose_remaining: []
plain_text_code_comments_translated: true
allowed_english_whitelist_documented: true
phrase_level_report_location: "внешний архив Round 4; заменён финальным Round 5 archive для acceptance"
result: pass
```

Разрешённый английский после Round 4: имена файлов и папок, ключи JSON/YAML, точные enum values, названия режимов `Service Mode` и `Execution Mode`, repository/commit/branch/PR tokens, а также технические токены с русским смыслом рядом.

## Закрытые problem IDs

```yaml
closed_problem_ids: [P-001,P-002,P-003,P-004,P-005,P-006,P-007,P-008,P-009,P-010,P-011,P-013,P-014,P-015,P-016,P-017,P-018,P-019,P-020,P-021,P-022,P-023,P-024,P-025,P-026,P-027,P-028]
p_012_verification_trace_closed: true
round2_closed_problem_ids: [R2-001,R2-002,R2-003,R2-004,R2-005]
round3_closed_problem_ids: [R3-001,R3-002,R3-003]
round4_closed_problem_ids: [R4-001,R4-002,R4-003]
round5_closed_problem_ids: [R5-001,R5-002,R5-003]
scope_creep_added: false
final_check_status: pass_after_round5_repair
next_expected_step: wait_for_user_request_or_open_service_issue
```

## Оставшиеся issues

Рабочих blocker-ов больше нет в утверждённом реестре Phase 1, Round 2, Round 3, Round 4 или Round 5. Новый финальный архив возвращается вне production repo; archive/handoff/evidence files в production repo не добавлялись.
