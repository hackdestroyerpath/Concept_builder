# Execution Mode protocol

[Назад к README](../../README.md)

## Назначение

Primary protocol для создания, продолжения и экспорта пользовательских концепций в `Concepts/`. System files обслуживаются через `Service Mode`, не через этот режим.

## Связанные файлы

- [Startup protocol](../common/startup.md)
- [Focus packet](../common/focus_packet.md)
- [Issue lifecycle](../issue/issue_lifecycle.md)
- [Concept release](../release/concept.md)
- [Concept template](../../Templates/concept/README.md)
- [Concepts root](../../Concepts/root.md)

## Startup cases

- `no_active`: показать actions `создать концепцию`, `открыть список концепций`, `восстановить focus`.
- `active_known`: загрузить concept `state.json`, `README.md`, `manifest.jsonl`, `structure.md`, локальный registry и active protocols.
- `active_unknown`: выполнить focus-loss recovery, не создавать новую концепцию автоматически.

## Concept creation model

Новая концепция создаётся только по реальному запросу пользователя. Сначала создаётся skeleton, затем concept becomes ready only after required content, manifest, structure, state and link network are consistent.

Минимальный file set:

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
pages/
```

`state.json` обязателен. Его отсутствие блокирует readiness и export.

## Concept state fields

Concept state follows [State schema](../../State/state_schema.md) and must include `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers`.

## Focus hierarchy

Focus order: execution index → active concept → concept section/page → concept issue → output. Agent may drop lower focus only after summary propagation to parent. Parent anchor must be written in focus packet.

## Concept issue workflow

Local registry row in `Concepts/<slug>/Issues/registry.jsonl` uses same lifecycle fields plus:

```json
{
  "concept_slug":"<slug>",
  "allowed_scope":"concept_only",
  "manifest_update_required":true,
  "structure_update_required":true,
  "concept_state_update_required":true,
  "service_escalation_required":false
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
```

## Export route

Export uses [Concept release](../release/concept.md). Draft export can include open nonblocking issues with snapshot. Final export is blocked by open blocking issues, missing state, broken links, orphan files, manifest/structure mismatch or failed language gate.
