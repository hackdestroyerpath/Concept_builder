# Context loading

[Назад к README](../../README.md)

## Назначение

Протокол минимальной загрузки контекста для `Service Mode`, `Execution Mode`, concept и issue focus. Primary schema находится в [focus_packet.md](focus_packet.md).

## Связанные файлы

- [Focus packet](focus_packet.md)
- [Startup protocol](startup.md)
- [Repository file index](../../Repository/file_index.jsonl)
- [Repository link graph](../../Repository/link_graph.md)

## Минимальный startup

1. `README.md`.
2. `Repository/file_index.jsonl`.
3. `Repository/link_graph.md`.
4. Relevant top-level state.
5. Protocol paths из `active_protocols`.
6. Focus files из `allowed_context`.

Весь repository tree, все concepts, все issues и вложения не читаются без конкретного `reload_reason`.

## Lean gate

```yaml
file_has_clear_function: true
primary_source_known: true
parent_known: true
reachable_from_entry: true
index_or_manifest_update_known: true
duplicate_risk_checked: true
language_gate_known: true
```

Если gate не проходит, production-файл не создаётся. Для `Concepts/<slug>/` обновляются local manifest, structure, concept state и link network.

## Context escalation

Расширение разрешено только по причине: `missing_source`, `broken_link`, `state_conflict`, `low_confidence`, `user_request`, `export_precheck`, `issue_resume`. Причина записывается в focus packet.

## Focus recovery

Если state загружен, но нельзя определить `current_entity_id`, parent anchor, active state files или next step, работа блокируется до recovery по [focus_packet.md](focus_packet.md).
