# Выпуск концепции

[Назад к Execution Mode](../execution/execution_mode.md)

## Назначение

Основной протокол для draft/final export, закрытия концепции, проверки link network и export report.

## Связанные файлы

- [Execution Mode](../execution/execution_mode.md)
- [Шаблон концепции](../../Templates/concept/README.md)
- [Корень Concepts](../../Concepts/root.md)
- [Финальная проверка](../../Checks/final.md)

## Команды export

- `export precheck` — dry-run без создания package.
- `draft export` — разрешён при open nonblocking issues; снимок перечисляет их и limitations.
- `final export` — разрешён только без open blocking issues и при успешных closure gates.

## Схема отчёта предварительной проверки

```yaml
concept_slug: string
export_type: precheck|draft|final
concept_state_current: true|false
manifest_current: true|false
structure_current: true|false
open_issues_snapshot: []
blocking_open_issues: []
nonblocking_open_issues: []
local_links_checked: true|false
backlinks_checked: true|false
orphan_files: []
language_gate: pass|fail
export_target: archive|markdown_network
package_name: string|null
failure_behavior: block|draft_with_notice|repair_required
```

## Политика draft/final

Draft export может выполняться с open nonblocking issues, если report содержит точный снимок issue и limitations. Final export блокируется при любом blocking issue, missing output, broken link, orphan file, manifest/structure mismatch, invalid state hash или failed language gate; рядом эти токены обозначают проверочные причины блокировки.

Пользовательское переопределение (`User override`) не может превратить blocking issue в final export. Оно может создать только non-final draft export.

## Имена package и metadata

```text
<concept_slug>__draft__YYYYMMDD_HHMMSS.zip
<concept_slug>__final__YYYYMMDD_HHMMSS.zip
```

Metadata хранится в export report:

```yaml
archive_name: string
created_at: ISO-8601
source_commit_sha: string
included_files: []
excluded_files: []
open_issues_snapshot: []
validation_summary: {}
```

## Содержимое package

Package включает только concept-local production files: README, pages, manifest, structure, state, relevant outputs и allowed attachments. Он исключает служебные рабочие файлы, implementation notes, unrelated concepts, handoff archives и task-state archives.

## Список закрытия concept

```yaml
readme_forward_links: true
child_page_backlinks: true
relative_links_open_locally: true
manifest_structure_mirror: true
local_issue_registry_checked: true
open_blocking_issues: []
language_gate_passed: true
export_report_written: true
concept_state_export_fields_updated: true
```

## Локальная проверка открытия

Перед export нужно распаковать или имитировать package root и открыть `README.md`. Каждая относительная ссылка из README и дочерних pages должна разрешаться внутри package. Сломанная локальная ссылка (`Broken local-open link`) блокирует final export и переводит draft export в `draft_with_notice`.

## Export report

Export report содержит машинные поля: commit SHA, export type, package name, included files, excluded files, open issues snapshot, validation checks, residual risks, next step и concept state update result.

## Обновление state

После export обнови concept `state.json`: `export_status`, `last_export_report`, `last_exported_at`, `last_export_package`, `open_issues_snapshot`, `readiness_status`, `next_expected_step`. Ошибка сохранения блокирует завершение export.
