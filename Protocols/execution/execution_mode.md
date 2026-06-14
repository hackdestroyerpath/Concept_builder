# Execution Mode protocol

[Назад к README](../../README.md)

## Назначение

Основной protocol для создания, продолжения, issue workflow и export пользовательских концепций в `Concepts/`. System files обслуживаются через `Service Mode`, не через этот режим.

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
| `active_known` | загрузить concept `state.json`, `README.md`, `manifest.jsonl`, `structure.md`, локальный registry и active protocols |
| `active_unknown` | выполнить focus-loss recovery, не создавать новую концепцию автоматически |

## Available actions menu

```text
1 — создать новую концепцию из пользовательского запроса
2 — продолжить активную концепцию
3 — открыть Concepts/root.md
4 — создать issue внутри активной концепции
5 — проверить готовность к export
6 — восстановить focus
```

Если active concept отсутствует, actions 2, 4 и 5 недоступны.

## Concept creation model

Новая концепция создаётся только по реальному запросу пользователя. Сначала создаётся initial skeleton, затем concept becomes ready только после согласованности required content, manifest, structure, state и link network.

## Минимальный file set для реальной concept folder

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
pages/              # только с реальными страницами
```

`state.json` обязателен. Его отсутствие блокирует readiness и export.

## Skeleton vs ready concept

| State | Allowed | Blocked |
|---|---|---|
| skeleton | collect requirements, draft pages, create local issue registry | final export, readiness claim |
| draft | iterate pages, resolve issues, run link checks | final export with blockers |
| ready | export precheck, draft/final export | mutation without issue/contract |

Ready требует README forward links, child backlinks, manifest/structure mirror, valid local registry, valid concept state hash, русские readable files и отсутствие blocking open issues.

## Concept state fields

Concept state follows [State schema](../../State/state_schema.md) и включает `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers`.

## Focus hierarchy

```text
execution index -> active concept -> page group -> page -> concept issue -> output
```

Agent может drop lower focus только after summary propagation to parent. Parent anchor записывается в focus packet.

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

Concept issue может менять только files внутри своего concept folder, кроме update верхнего execution index state. Если найден system defect, агент создаёт service issue вместо editing system files.

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

Failure блокирует closure и export.

## Slug and first registry gate

Перед созданием `Concepts/<slug>/` нужно confirm или derive stable slug. First write creates concept `README.md`, `state.json`, `manifest.jsonl`, `structure.md` и `Issues/registry.jsonl` together in one persistence plan. Empty decorative pages не создаются.

## Export route

Export uses [Concept release](../release/concept.md). Draft export может include open nonblocking issues with snapshot. Final export блокируется open blocking issues, missing state, broken links, orphan files, manifest/structure mismatch или failed language gate.
