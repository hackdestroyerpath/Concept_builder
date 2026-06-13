# Concept release

[Назад к Execution Mode](../execution/execution_mode.md)

## Назначение

Primary protocol для draft/final export, concept closure, link network validation и export report.

## Связанные файлы

- [Execution Mode](../execution/execution_mode.md)
- [Concept template](../../Templates/concept/README.md)
- [Concepts root](../../Concepts/root.md)
- [Final check](../../Checks/final.md)

## Export commands

- `draft export` — разрешён при open nonblocking issues; snapshot must list them.
- `final export` — разрешён только без open blocking issues and with all closure gates pass.
- `export precheck` — dry-run без создания package.

## Precondition report

```yaml
concept_slug: string
export_type: draft|final
concept_state_current: true
manifest_current: true
structure_current: true
open_issues_snapshot: []
blocking_open_issues: []
local_links_checked: true
backlinks_checked: true
orphan_files: []
language_gate: pass|fail
package_name: string
```

## Draft/final policy

Draft export may proceed with open nonblocking issues if report includes exact issue snapshot and limitations. Final export is blocked by any blocking issue, missing output, broken link, orphan file, manifest/structure mismatch or failed language gate.

## Package contents

Package includes only concept-local production files: README, pages, manifest, structure, state, relevant outputs and allowed attachments. It excludes service work files, implementation notes and unrelated concepts.

## Concept closure checklist

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

## Export report

Export report contains commit SHA, export type, package name, included files, excluded files, open issues snapshot, validation checks, residual risks, next step and concept state update result.

## State update contract

After export update concept `state.json`: `export_status`, `last_export_report`, `last_exported_at`, `last_export_package`, `readiness_status`, `next_expected_step`. Persistence failure blocks export completion.
