# Протокол Execution Mode

[Назад к README](../../README.md)

## Назначение

Основной протокол для пользовательских концепций в `Concepts/`: создание, продолжение, локальные issue и export. Служебные файлы обслуживаются через `Service Mode`.

## Связанные файлы

- [Протокол запуска](../common/startup.md)
- [Пакет фокуса](../common/focus_packet.md)
- [Жизненный цикл issue](../issue/issue_lifecycle.md)
- [Выпуск концепции](../release/concept.md)
- [Шаблон концепции](../../Templates/concept/README.md)
- [Корень Concepts](../../Concepts/root.md)

## Запуск

`no_active` показывает действия для создания концепции, открытия списка или восстановления focus. `active_known` загружает `state.json`, `README.md`, `manifest.jsonl`, `structure.md`, локальный registry и `active_protocols`. `active_unknown` запускает восстановление фокуса.

## Создание концепции

Новая концепция создаётся только по реальному запросу пользователя. Сначала создаётся начальный каркас (`initial skeleton`). Concept считается ready после согласования required content, manifest, structure, state и link network; эти токены являются именами проверяемых частей.

Минимальный набор файлов: `README.md`, `about.md`, `operating_model.md`, `requirements.md`, `process.md`, `state.json`, `manifest.jsonl`, `structure.md`, `Issues/registry.jsonl`. Папка `pages/` создаётся только для реальных страниц.

## Готовность

`state.json` обязателен. Skeleton concept не считается ready. Ready требует forward links из README, backlinks дочерних страниц, mirror manifest/structure, valid local registry, valid concept state hash, русские readable files и отсутствие blocking open issues.

## Поля concept state

Concept state следует [State schema](../../State/state_schema.md) и включает `concept_slug`, `active_issue_id`, `readiness_status`, `export_status`, `last_export_report`, `manifest_path`, `structure_path`, `local_issue_registry`, `focus_pointers`.

## Concept issue

Локальная строка registry в `Concepts/<slug>/Issues/registry.jsonl` использует поля общего жизненного цикла и поля `concept_slug`, `allowed_scope`, `manifest_update_required`, `structure_update_required`, `concept_state_update_required`, `service_escalation_required`.

Concept issue меняет только файлы внутри concept folder, кроме update верхнего execution index state. Системный дефект оформляется как service issue.

## Проверка manifest и structure

Любое создание, удаление, переименование страницы или изменение ссылки внутри concept требует `manifest_updated`, `structure_updated`, `local_links_checked`, `backlinks_checked`, `concept_state_updated` и `local_registry_updated`.

## Маршрут export

Export использует [Concept release](../release/concept.md). Draft export может включать open nonblocking issues со снимком. Final export останавливается при open blocking issues, missing state, broken links, orphan files, manifest/structure mismatch или failed language gate; эти токены являются названиями проверочных причин.
