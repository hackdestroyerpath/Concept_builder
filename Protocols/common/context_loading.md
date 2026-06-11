# Context loading

[Назад к README](../../README.md)

## Назначение

Протокол задаёт минимальную загрузку контекста для `Service Mode` и `Execution Mode`.
Цель — открыть только нужные файлы, восстановить focus и не превращать каждый старт в раскопки всего репозитория.

## Минимальный пакет

При старте агент открывает:

1. `README.md`.
2. `Repository/file_index.jsonl`.
3. `Repository/link_graph.md`.
4. Relevant state: `State/service_state.json` или `State/execution_index_state.json`.
5. Protocol-файлы из `active_protocols` relevant state.
6. Файлы текущего focus, если focus задан.

## Focus packet

Focus packet должен содержать:

```yaml
mode: service|execution
state_file: path
current_focus: string|null
focus_stack: []
active_protocols: []
allowed_context: []
blocked_context: []
next_expected_step: string
context_confidence: high|medium|low
```

## Запрещённый контекст по умолчанию

Не загружать без явной причины:

- implementation archive;
- checkpoint archives;
- temporary notes;
- все concepts сразу;
- все active issue сразу;
- attachments, если они не являются source текущего issue.

## Эскалация контекста

Расширять контекст можно только если текущего focus packet недостаточно.
Перед расширением агент фиксирует причину: missing source, broken link, conflict in state, low confidence или user request.

## Gate перед изменением файла

```yaml
file_has_clear_function: true
primary_source_known: true
parent_known: true
reachable_from_entry: true
index_or_manifest_update_known: true
duplicate_risk_checked: true
```

Если gate не проходит, файл не создаётся и не изменяется.
