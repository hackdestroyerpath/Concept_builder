# Карта связей репозитория

[Назад к README](../README.md)

## Назначение

Карта достижимости активных рабочих файлов, договор навигации Markdown и воспроизводимая проверка ссылок, сиротских файлов и языка. Машинный список путей хранится в [file_index.jsonl](file_index.jsonl); этот файл объясняет, как этот список проверять.

## Связанные файлы

- [README](../README.md)
- [Индекс файлов](file_index.jsonl)
- [Финальная проверка](../Checks/final.md)

## Активное рабочее дерево

```text
README.md
Checks/final.md
Concepts/root.md
Inbox/README.md
Instructions/concept_builder_project_instruction.md
Instructions/concept_builder_service_mode_project_instruction.md
Issues/registry.jsonl
Protocols/common/context_loading.md
Protocols/common/focus_packet.md
Protocols/common/startup.md
Protocols/common/state_update.md
Protocols/execution/execution_mode.md
Protocols/issue/complex_linked.md
Protocols/issue/issue_lifecycle.md
Protocols/release/concept.md
Protocols/service/input_registry.md
Protocols/service/service_mode.md
Repository/file_index.jsonl
Repository/link_graph.md
State/execution_index_state.json
State/service_state.json
State/state_schema.md
Templates/concept/README.md
Templates/issue/README.md
```

Все эти пути должны открываться через GitHub Connector на целевой ветке. Папки `Issues/active/`, `Inbox/<input_id>/` и `Concepts/<slug>/` создаются только по реальным запросам и после обновления соответствующего реестра или manifest.

## Корневой маршрут

- [README](../README.md)
  - [Индекс файлов](file_index.jsonl)
  - [Финальная проверка](../Checks/final.md)
  - [Схема состояния](../State/state_schema.md)
  - [Протокол запуска](../Protocols/common/startup.md)
  - [Service Mode](../Protocols/service/service_mode.md)
  - [Execution Mode](../Protocols/execution/execution_mode.md)

## Рабочие маршруты

| Маршрут | Файлы |
|---|---|
| Состояние | [service](../State/service_state.json), [execution](../State/execution_index_state.json), [schema](../State/state_schema.md) |
| Инструкции | [execution](../Instructions/concept_builder_project_instruction.md), [service](../Instructions/concept_builder_service_mode_project_instruction.md) |
| Общие протоколы | [startup](../Protocols/common/startup.md), [context](../Protocols/common/context_loading.md), [focus](../Protocols/common/focus_packet.md), [state update](../Protocols/common/state_update.md) |
| Service | [mode](../Protocols/service/service_mode.md), [input registry](../Protocols/service/input_registry.md), [issue registry](../Issues/registry.jsonl), [inbox](../Inbox/README.md) |
| Execution | [mode](../Protocols/execution/execution_mode.md), [concepts](../Concepts/root.md), [release](../Protocols/release/concept.md), [concept template](../Templates/concept/README.md) |
| Issues | [lifecycle](../Protocols/issue/issue_lifecycle.md), [complex linked](../Protocols/issue/complex_linked.md), [issue template](../Templates/issue/README.md) |
| Checks | [final evidence](../Checks/final.md) |

## Договор навигации Markdown

Каждый активный Markdown-файл, кроме корневого README, обязан иметь:

1. один H1;
2. обратную ссылку к родителю или корневому маршруту;
3. раздел `Назначение`;
4. раздел `Связанные файлы`, если есть локальные зависимости;
5. примечание об основном источнике или ссылку на него;
6. только относительные ссылки внутри репозитория;
7. поведение при ошибке, если файл задаёт проверку или рабочий процесс.

Корневой README является исключением: вместо обратной ссылки он содержит краткую карту, маршруты, правила целостности и следующие действия.

## Ожидания по обратным ссылкам

- Любой файл, указанный в поле `parent`, должен иметь маршрут из README или рабочих маршрутов.
- Сводки и шаблоны ссылаются на основной источник, но не вводят альтернативную схему.
- Экземпляры задач и концепций обязаны иметь локальную обратную ссылку к родителю и строку в реестре или manifest.
- `output/report.md` связан с состоянием задачи и строкой реестра; имя `output_report.md` запрещено.

## Процедура проверки ссылок, сирот и языка

1. Распарсить [file_index.jsonl](file_index.jsonl) как JSONL; каждая строка должна иметь `path`, `kind`, `owner_mode`, `purpose`, `parent`, `primary_source`, `described_in`, `status`.
2. Получить снимок рабочего дерева через доступный GitHub-маршрут и отдельно перечитать каждый индексированный файл через GitHub Connector.
3. Сравнить физические пути с путями `status=active` из индекса.
4. Для каждого Markdown-файла извлечь относительные ссылки на `.md`, `.json` и `.jsonl`; целевой путь должен существовать в активном индексе или быть допустимым будущим путём внутри экземпляра задачи или концепции.
5. Проверить достижимость от корня: README → маршрут → файл. Файл без маршрута считается сиротским.
6. Проверить обратные ссылки: дочерние файлы и сводки возвращаются к родителю или основному источнику.
7. Проверить служебные слова `handoff`, `phase1_audit`, `task-state`, `implementation_report`, `checkpoint`, `temporary_notes`, `original_handoff`. Совпадения допустимы только как запретительные правила или доказательства в финальной проверке, не как рабочие пути.
8. Проверить языковой барьер: читаемый Markdown пишется по-русски; английский допустим только для путей, режимов, машинных значений и необходимых технических токенов с русским смыслом рядом.
9. Результат записать в [Checks/final.md](../Checks/final.md) с набором входов, методом, результатом, исключениями и правилом повтора.

## Снимок проверки Round 4

```yaml
round4_baseline_main_commit: "db0bab94418165a1d9c9302d43977b3cee7d6b8b"
round4_work_branch: "agent/20260614-round4-language-gate"
validation_target: "main после PR merge"
indexed_active_files: 24
active_markdown_files_scanned: 20
language_cleanup_scope: "все активные читаемые Markdown-файлы"
new_production_files_added_by_round4: []
production_files_deleted_by_round4: []
dev_only_files_expected_in_production: []
final_evidence_file: "Checks/final.md"
external_archive_required: true
```
