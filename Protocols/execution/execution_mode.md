# Execution Mode protocol

[Назад к README](../../README.md)

## Назначение

Primary protocol для создания, продолжения, issue workflow and export пользовательских концепций в `Concepts/`. System files обслуживаются через `Service Mode`, не через этот режим.

## Связанные файлы

- [Startup protocol](../common/startup.md)
- [Focus packet](../common/focus_packet.md)
- [Issue lifecycle](../issue/issue_lifecycle.md)
- [Concept release](../release/concept.md)
- [Concept template](../../Templates/concept/README.md)
- [Concepts root](../../Concepts/root.md)

## Startup cases

| Case | Action |
|---|---|
| `no_active` | показать actions `создать концепцию`, `открыть список концепций`, `восстановить focus` |
| `active_known` | загрузить concept `state.json`, `README.md`, `manifest.jsonl`, `structure.md`, локальный registry and active protocols |
| `active_unknown` | выполнить focus-loss recovery, не создавать новую концепцию automatically |

## Available actions menu

```text
1 — создать новую концепцию из пользовательского запроса
2 — продолжить активную концепцию
3 — открыть Concepts/root.md
4 — создать issue внутри активной концепции
5 — проверить готовность к export
6 — восстановить focus
```

Если active concept отсутствует, actions 2, 4 and 5 недоступны.

## Concept creation model

Новая концепция создаётся только по реальному запросу пользователя. Сначала создаётся initial skeleton, затем concept becomes ready only after required content, manifest, structure, state and link network are consistent.

Минимальный file set для реальной concept folder:

```text
README.md
about.md
operating_model.md
requirements.md
process.md
state.json
manifest.jsonl
structure.md
Issues/registry.jsonl
pages/              # only with real pages
```

`state.json` обязателен. Его отсутствие блокирует readiness and export.

## Skeleton vs ready concept

| State | Allowed | Blocked |
|---|---|---|
| skeleton | collect requirements, draft pages, create local issue registry | final export, readiness claim |
| draft | iterate pages, resolve issues, run link checks | final export with blockers |
| ready | export precheck, draft/final export | mutation without issue/contract |

Ready requires README forward links, child backlinks, manifest/structure mirror, local registry valid, concept state hash valid, Russian readable files, and no blocking open issues.

## Concept state fields

Concept state follows [State schema](../../State/state_schema.md) and includes `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers`.

## Focus hierarchy

```text
execution index -> active concept -> page group -> page -> concept issue -> output
```

Agent may drop lower focus only after summary propagation to parent. Parent anchor must be written in focus packet.

## Concept issue workflow

Local registry row in `Concepts/<slug>/Issues/registry.jsonl` uses service lifecycle fields plus:

```json
{
  "concept_slug": "<slug>",
  "allowed_scope": "concept_only",
  "manifest_update_required": true,
  "structure_update_required": true,
  "concept_state_update_required": true,
  "service_escalation_required": false
}
```

Concept issue may mutate only files inside its concept folder, except execution index state update. If it discovers system defect, create service issue instead of editing system files.

## Manifest/structure gate

Any page creation, deletion, rename or link change inside concept requires:

```yaml
manifest_updated: true
structure_updated: true
local_links_checked: true
backlinks_checked: true
concept_state_updated: true
local_registry_updated: true
```

Failure blocks closure and export.

## Slug and first registry gate

Before creating `Concepts/<slug>/`, confirm or derive stable slug. First write creates concept `README.md`, `state.json`, `manifest.jsonl`, `structure.md` and `Issues/registry.jsonl` together in one persistence plan. Empty decorative pages are not created.

## Export route

Export uses [Concept release](../release/concept.md). Draft export can include open nonblocking issues with snapshot. Final export is blocked by open blocking issues, missing state, broken links, orphan files, manifest/structure mismatch or failed language gate.
