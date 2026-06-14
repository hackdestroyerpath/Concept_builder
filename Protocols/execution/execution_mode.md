# Протокол Execution Mode

[Назад к README](../../README.md)

## Назначение

Основной протокол для создания, продолжения, рабочего процесса issue и export пользовательских концепций в `Concepts/`. Системные файлы обслуживаются через `Service Mode`, не через этот режим.

## Связанные файлы

- [Протокол запуска](../common/startup.md)
- [Пакет фокуса](../common/focus_packet.md)
- [Жизненный цикл issue](../issue/issue_lifecycle.md)
- [Выпуск концепции](../release/concept.md)
- [Шаблон концепции](../../Templates/concept/README.md)
- [Корень Concepts](../../Concepts/root.md)

## Случаи запуска

| Случай | Действие |
|---|---|
| `no_active` | показать действия `создать концепцию`, `открыть список концепций`, `восстановить focus` |
| `active_known` | загрузить concept `state.json`, `README.md`, `manifest.jsonl`, `structure.md`, локальный registry и active protocols |
| `active_unknown` | выполнить восстановление фокуса, не создавать новую концепцию автоматически |

## Меню доступных действий

```text
1 — создать новую концепцию из пользовательского запроса
2 — продолжить активную концепцию
3 — открыть Concepts/root.md
4 — создать issue внутри активной концепции
5 — проверить готовность к export
6 — восстановить focus
```

Если active concept отсутствует, действия 2, 4 и 5 недоступны.

## Модель создания концепции

Новая концепция создаётся только по реальному запросу пользователя. Сначала создаётся initial skeleton, то есть начальный каркас; затем concept считается ready только после согласованности required content, manifest, structure, state и link network.

## Минимальный набор файлов реальной concept folder

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

## Skeleton и ready concept

| State | Разрешено | Заблокировано |
|---|---|---|
| skeleton | собирать requirements, черновить pages, создавать local issue registry | final export и readiness claim |
| draft | дорабатывать pages, закрывать issues, запускать link checks | final export при blockers |
| ready | export precheck, draft/final export | mutation без issue/contract |

Ready требует forward links из README, backlinks дочерних страниц, mirror manifest/structure, valid local registry, valid concept state hash, русские readable files и отсутствие blocking open issues.

## Поля concept state

Concept state следует [State schema](../../State/state_schema.md) и включает `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers`.

## Иерархия фокуса

```text
execution index -> active concept -> page group -> page -> concept issue -> output
```

Agent может сбрасывать нижний focus только после переноса summary к parent. Parent anchor записывается в focus packet.

## Рабочий процесс concept issue

Local registry row в `Concepts/<slug>/Issues/registry.jsonl` использует поля service lifecycle плюс:

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

Concept issue может менять только файлы внутри concept folder, кроме update верхнего execution index state. Если найден system defect, агент создаёт service issue вместо прямого изменения system files.

## Проверка manifest и structure

Любое создание, удаление, переименование страницы или изменение ссылки внутри concept требует:

```yaml
manifest_updated: true
structure_updated: true
local_links_checked: true
backlinks_checked: true
concept_state_updated: true
local_registry_updated: true
```

Сбой проверки блокирует closure и export.

## Slug и первый registry

Перед созданием `Concepts/<slug>/` нужно подтвердить или вывести стабильный slug. Первая запись создаёт `README.md`, `state.json`, `manifest.jsonl`, `structure.md` и `Issues/registry.jsonl` вместе в одном плане сохранения. Пустые декоративные pages не создаются.

## Маршрут export

Export использует [Concept release](../release/concept.md). Draft export может включать open nonblocking issues со снимком. Final export блокируется при open blocking issues, missing state, broken links, orphan files, manifest/structure mismatch или failed language gate; эти токены являются названиями проверочных причин.
