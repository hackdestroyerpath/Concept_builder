# Финальная проверка

[Назад к карте связей](../Repository/link_graph.md)

## Назначение

Финальная проверка на основе evidence, то есть доказательств, после Phase 2, Round 2 и узкого Round 3 language-gate repair. Round 3 закрывает только `R3-001`–`R3-003`: языковой барьер, доказательство языковой проверки и обновление service state. Полный редизайн не выполнялся.

## Связанные файлы

- [README](../README.md)
- [Индекс файлов](../Repository/file_index.jsonl)
- [Карта связей](../Repository/link_graph.md)
- [Схема состояния](../State/state_schema.md)
- [Состояние service](../State/service_state.json)

## Снимок доказательств Round 3

```yaml
round3_baseline_main_commit: "49fc06c174ad021deb1e85998e3ecab41d26e584"
round3_work_branch: "agent/20260614-round3-language-gate"
validation_target: "main after PR merge"
default_branch: "main"
indexed_active_files: 24
active_readable_markdown_scanned: 20
new_production_files_added: []
production_files_deleted: []
external_archives_uploaded_to_production: false
current_main_commit_after_round3: "recorded in returned final evidence archive and final response after merge"
```

## Активные Markdown-файлы, просканированные в Round 3

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

## Изменённые рабочие файлы Round 3

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
State/service_state.json
```

## Round 3 closure matrix

| ID | Область | Требуемое исправление | Доказательство | Результат | Остаточный риск |
|---|---|---|---|---|---|
| R3-001 | Язык читаемого Markdown | Убрать обычную английскую прозу во всех активных Markdown-файлах | Скан всех активных Markdown-файлов и внешний пофразовый отчёт | closed | Английский оставлен только для путей, ключей, режимов и технических токенов с русским смыслом рядом |
| R3-002 | Финальные доказательства | Дать проверяемый отчёт вместо самозаявленного pass | Этот файл и внешний архив с language_gate_report | closed | Детальный отчёт до/после хранится во внешнем архиве, чтобы не возвращать старую английскую прозу в рабочий Markdown |
| R3-003 | Сохранение state | Обновить `State/service_state.json`, сохранить `allowed_exception: null`, пересчитать hash | Блок JSON/hash ниже | closed | Нет активного исключения |

## Сводка приёмки PH2/R2

| Проверка | Результат | Указатель доказательств | Остаточный риск |
|---|---|---|---|
| Маршрутизация README | pass | `README.md` | Нет |
| Авторитет пакета фокуса | pass | `Protocols/common/focus_packet.md` | Нет |
| Service/input registry | pass | `Protocols/service/input_registry.md`; `Issues/registry.jsonl` | Пустой registry допустим до первой issue |
| Жизненный цикл issue и путь output | pass | `Protocols/issue/issue_lifecycle.md`; `Templates/issue/README.md` | Проверка реальной issue ждёт первую issue |
| Сложный/связанный процесс issue | pass | `Protocols/issue/complex_linked.md` | Активного графа зависимостей нет |
| Модель execution/concept | pass | `Protocols/execution/execution_mode.md`; `Templates/concept/README.md`; `Concepts/root.md` | Реальной concept пока нет |
| Договор release/export | pass | `Protocols/release/concept.md` | Реальный архив ждёт реальную concept |
| Смысл state/hash | pass | `State/state_schema.md`; блок hash ниже | Финальный commit SHA хранится вне self-hash |
| Проверка link/orphan | pass | `Repository/link_graph.md` | API tree endpoint не показан connector-ом |
| Языковой барьер после Round 3 | pass | раздел языка ниже и внешний архив | Нет |
| Размер инструкций проекта | pass | блок размера инструкций ниже | Ручное обновление настроек проекта, если копия устарела |

## Проверки моделированием

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
service_state_hash: "sha256:bbdb9b910d63682577e76af065019518382caffe694b1521d6041117a23a5282"
execution_state_hash: "sha256:c04bb0727bb28822af03ae1fe98ef32923fc66495d45106bf70552a501e5be6e"
state_hash_pending_values: []
issue_registry_jsonl_valid_empty: true
file_index_jsonl_active_records: 24
file_index_required_fields_present: true
```

## Отчёт проверки ссылок и сирот

```yaml
input_set: "Repository/file_index.jsonl status=active"
indexed_active_files: 24
active_readable_markdown_scanned: 20
indexed_active_files_missing: []
physical_files_not_in_file_index: []
root_reachability: pass
relative_markdown_links: pass
backlink_contract: pass
orphan_indexed_files: []
dev_only_files_in_production: []
allowed_mentions_of_dev_terms: "только запретительные правила или доказательства"
```

## Языковой барьер Round 3

```yaml
default_readable_language: "ru"
all_active_markdown_scanned: true
nontechnical_english_prose_remaining: []
code_blocks_machine_values_preserved: true
allowed_english_whitelist_documented: true
phrase_level_report_location: "external archive: workspace/language_gate_report.md"
result: pass
```

Разрешённый английский после Round 3:

- имена файлов и папок;
- ключи JSON/YAML и машинные значения в code blocks;
- режимы `Service Mode` и `Execution Mode` как названия режимов;
- технические токены `state`, `issue`, `registry`, `manifest`, `export`, `hash`, `README`, `commit`, `branch`, `PR`, если рядом есть русский смысл или они находятся в машинном контексте.

## Сводка пофразовых доказательств

Детальный список до/после хранится во внешнем архиве, чтобы не возвращать обычную английскую прозу в рабочий Markdown. В рабочем репозитории зафиксированы только русские описания категорий и ссылки на изменённые файлы.

| Категория | Пример исправления на русском | Результат |
|---|---|---|
| Заголовки процесса | `Workflow` заменён русским заголовком `Рабочий процесс` | pass |
| Служебные исключения | `Narrow exception` заменён русским описанием узкого исключения | pass |
| Пакеты и export | Обычные английские предложения про package/export переведены на русский | pass |
| Распространение parent/child | Обычные английские глаголы и связки заменены русскими формулировками | pass |
| State/hash | Обычная английская проза вокруг state/hash переведена; ключи JSON сохранены | pass |

## Размер инструкций проекта

```yaml
concept_builder_project_instruction_chars: 2019
concept_builder_service_mode_project_instruction_chars: 1906
limit_chars_each: 8000
under_limit_each: true
```

## Договор финального архива

```yaml
final_archive_contains_required_files: true
final_archive_matches_repo_state: true
no_archive_uploaded_to_production: true
current_main_commit_recorded_after_merge: true
```

## Закрытые problem IDs

```yaml
closed_problem_ids: [P-001,P-002,P-003,P-004,P-005,P-006,P-007,P-008,P-009,P-010,P-011,P-013,P-014,P-015,P-016,P-017,P-018,P-019,P-020,P-021,P-022,P-023,P-024,P-025,P-026,P-027,P-028]
p_012_verification_trace_closed: true
round2_closed_problem_ids: [R2-001,R2-002,R2-003,R2-004,R2-005]
round3_closed_problem_ids: [R3-001,R3-002,R3-003]
scope_creep_added: false
final_check_status: pass
next_expected_step: wait_for_user_request_or_open_service_issue
```

## Оставшиеся issues

Рабочих blocker-ов больше нет в утверждённом реестре Phase 1, Round 2 или Round 3. Ручная заметка: исходники GitHub обновлены; если скопированные настройки ChatGPT Project содержат старый текст, обнови их из `Instructions/`.
