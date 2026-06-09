# Service Mode protocol

[Назад к README](../../README.md)

Связанные файлы:
- [Startup protocol](../common/startup.md)
- [Focus packet](../common/focus_packet.md)
- [State update](../common/state_update.md)
- [Service state](../../State/service_state.json)
- [Issue registry](../../Issues/registry.jsonl)
- [Repository file index](../../Repository/file_index.jsonl)
- [Repository link graph](../../Repository/link_graph.md)

## Назначение

`Service Mode` обслуживает сам `Concept Builder`: структуру репозитория, системные протоколы, верхние state-файлы, project instructions, registry и service-level issue.
Этот режим не ведёт пользовательские концепции как основную работу.

## Границы режима

Разрешено:

- менять `README.md`, `Repository/file_index.jsonl` и `Repository/link_graph.md`;
- создавать и обновлять project instruction sources;
- менять `State/service_state.json`, `State/execution_index_state.json` и `State/state_schema.md`;
- создавать, вести и закрывать service-level issue;
- создавать и изменять системные protocol-файлы;
- проверять broken links, orphan files, language consistency и contract coverage.

Запрещено:

- вести конкретную пользовательскую концепцию как основную работу;
- создавать demo concept folders без реального запроса;
- загружать ТЗ-архивы, checkpoint archives, temporary notes или отчёты реализации в рабочий GitHub;
- утверждать, что persistence выполнен, если GitHub-запись не прошла.

## Lean gate перед созданием файла

Перед созданием нового production-файла агент проверяет:

```yaml
file_has_clear_function: true
primary_source_known: true
parent_known: true
reachable_from_entry: true
index_update_required: true|false
duplicate_risk_checked: true
```

Если хотя бы одно поле не подтверждено, файл не создаётся. Мир и так полон лишних файлов, не будем кормить свалку.

## Workflow service issue

1. Принять запрос пользователя или internal repair need.
2. Классифицировать issue как `simple` или `complex`.
3. Для complex issue создать папку `Issues/active/<issue_id>/`.
4. Заполнить `reason.md`, `state.json`, requirements, plan, solution, contract и output, если они применимы.
5. Обновить `Issues/registry.jsonl`.
6. Выполнить изменение production-файлов.
7. Обновить `Repository/file_index.jsonl`, `Repository/link_graph.md` и relevant state.
8. Перед ответом пользователю проверить persistence.

## Service issue registry

`Issues/registry.jsonl` является primary source для верхних service-level issue.
Пустой registry допустим, пока active issue нет.

Минимальная строка registry:

```json
{"issue_id":"...","status":"open","type":"service","path":"Issues/active/.../state.json","title":"...","created_at":"...","updated_at":"..."}
```

## Закрытие service task

Service task можно закрыть, если:

```yaml
production_files_written: true
file_index_updated: true
link_graph_updated: true
state_updated: true
broken_active_links: []
orphan_active_files: []
user_response_ready: true
```

## Ответ пользователю

Ответ должен содержать:

- что изменено;
- какие файлы затронуты;
- commit sha, если доступен;
- текущий state flag;
- следующий допустимый шаг.
