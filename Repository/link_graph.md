# Repository link graph

[Назад к README](../README.md)

Связанные файлы:
- [File index](file_index.jsonl)
- [Issues registry](../Issues/registry.jsonl)
- [Inbox](../Inbox/README.md)
- [Concepts](../Concepts/root.md)

## Назначение

Этот файл фиксирует текущую карту достижимости рабочего репозитория и orphan-проверку Phase 1.
Он не заменяет `Repository/file_index.jsonl`: эта страница нужна для чтения человеком и агентом, а index остаётся машинным источником проверки.

## Активные узлы Phase 1

| Файл | Parent | Достижимость |
|---|---|---|
| `README.md` | `null` | root |
| `Repository/file_index.jsonl` | `README.md` | `README.md` -> `Repository/file_index.jsonl` |
| `Repository/link_graph.md` | `README.md` | `README.md` -> `Repository/link_graph.md` |
| `Issues/registry.jsonl` | `README.md` | `README.md` -> `Issues/registry.jsonl` |
| `Inbox/README.md` | `README.md` | `README.md` -> `Inbox/README.md` |
| `Concepts/root.md` | `README.md` | `README.md` -> `Concepts/root.md` |

## Плановые узлы, ещё не активные

Эти пути зарезервированы архитектурой, но не считаются ссылками Phase 1, пока файлы физически не созданы:

- `Instructions/concept_builder_project_instruction.md`
- `Instructions/concept_builder_service_mode_project_instruction.md`
- `State/service_state.json`
- `State/execution_index_state.json`
- `State/state_schema.md`
- `Protocols/common/startup.md`
- `Protocols/common/context_loading.md`
- `Protocols/common/state_update.md`
- `Protocols/service/service_mode.md`
- `Protocols/execution/execution_mode.md`

## Orphan-проверка Phase 1

```yaml
link_graph_status: pass
checked_scope: phase_1_created_files
orphan_files: []
broken_links: []
missing_backlinks: []
missing_manifest_entries: []
notes:
  - Все физически созданные файлы Phase 1 описаны в Repository/file_index.jsonl.
  - Плановые пути перечислены как code path, а не как Markdown-ссылки.
  - Issues/registry.jsonl допускается как пустой JSONL-файл до первого issue.
```
